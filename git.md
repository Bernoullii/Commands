## Error: 
`git@github.com: permission denied (publickey). fatal: could not read from remote repository. please make sure you have the correct access rights and the repository exists.`

## Fix: 
This means, on your local machine, you haven't made any SSH keys. Here's how to fix:

1. Open git bash (Use the Windows search. To find it, type "git bash") or the Mac Terminal.
2. Type `cd ~/.ssh`. This will take you to the root directory for Git (Likely `C:\Users\[YOUR-USER-NAME]\.ssh\` on Windows, `/home/[YOUR-USER-NAME]/.ssh/` on Ubuntu).
3. Within the `.ssh` folder, there should be these two files: `id_rsa` and `id_rsa.pub`. These are the files that tell your computer how to communicate with GitHub, BitBucket, or any other Git based service. Type `ls` to see a directory listing. If those two files don't show up, proceed to the next step. __NOTE:__ Your SSH keys __must__ __be__ __named__ `id_rsa` and `id_rsa.pub` in order for Git, GitHub, and BitBucket to recognize them by default.
4. To create the SSH keys, type `ssh-keygen -t rsa -C "your_email@example.com"`. This will create both `id_rsa` and `id_rsa.pub` files.
5. Now, go and open `id_rsa.pub` in your favorite text editor (you can do this via Windows Explorer or the OSX Finder if you like, typing `open .` will open the folder).
6. Copy the contents--exactly as it appears, with no extra spaces or lines--of `id_rsa.pub` and paste it into GitHub under the Account Settings > SSH Keys.
7. Now that you've added your public key to Github. try to `git fitch` or `git push` again and see if it works. It should!

More help available from [GitHub on creating SSH Keys](https://help.github.com/articles/generating-ssh-keys).
