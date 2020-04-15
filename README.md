# Mac-Rsync-Chron_Cloud
Sync dev files to the cloud without including Node Modules and more....

# Usage

##Confirm you have rsync

MacOS and most Linux distros should come preloaded with the rsync binary. 
Check that you have it by opening up a command prompt and running

```
which rsync
```

##Write the Sync Script
Edit the downloaded bash script backup.sh or whatever you choose to name it

Change the src_dir and dest_dir to the directories you’d like. Make sure to include trailing slashes! Our rsync setup needs this.

Please add or remove the --exclude args to your liking.

The a flags is a compound flag telling rsync to do a bunch of things that you don’t need to worry about. The r flags tells rsync to work recursively. The --delete flag tells rsync to delete files in dest_dir when they no longer exist in src_dir. I always do this because I don’t want dest_dir to accumulate old deleted files. If I want access to old deleted files, I can use Dropbox’s or Google Drive’s history.

The --filter=':- .gitignore' portion of the rsync command is really neat. It tells rsync to exclude the files that are listed in the .gitignore file in each directory. These are files you already don’t care about, such as vendor files or temp files, so you won’t be forced to create another list!

The --chmod='F-w' portion tells rsync that the copied files (but not directories) should have their write permissions removed. This is a good idea because it prevents us from accidentally going into the dest_dir instead of the authoritative src_dir and making edits. We unfortunately can’t do this for directories because we need to allow rsync to add new files and delete old ones.

##Make sure it has execution permission.
You can open command prompt and run..

```
chmod +x <path-to-file>
```
##Test the Sync Script

Let’s ensure the syncing script works. Open a prompt and execute it! 
You should see a newly created dest_dir that excludes all the node_modules directories and whatnot.

##Install the Cron Job
Open command prompt..
execute crontab -e or nano crontab -e to open up a text editor for editing your cron tasks. 

```
crontab -e
```
Add the line:

```
*/10 * * * * <path-to-sync-script>
```

This runs our sync script every 10 minutes. Change the 10 to a 5 to run it every 5 minutes for example. 
And of course make sure to write the absolute path to <path-to-sync-script>.

##Test the Cron Job

Verify the cron job was saved by running crontab -l to list installed jobs.

After installing the cron, try to make some modifications within your projects directory. 
Then, after the current 10-minute interval of time ends (if it’s 4:32, wait until 4:40), watch your files sync!

You may have to open system prefs on a mac to give cron "full disk access"



#Happy Scyncing !!
