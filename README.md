# LavaLust Framework Documentation

## Server Requirements
	* At least use PHP 7.2
	* MySQL 5 or higher
	* PDO is installed
	* mod_rewrite is enabled

## LavaLust is installed in four steps:
	1. Unzip the package or you can also clone its repository in Github
	2. Upload the LavaLust folders and files to your server.
	3. Open the application/config/config.php file with a text editor and set your base URL.
	4. If you intend to use a database, open the application/config/database.php file
	with a text editor and set your database settings..


	If you wish to increase security by hiding the location of your
	LavaLust files you can rename the system and application folders
	to something more private. If you do rename them, you must open
	your main index.php file and set the $system_path and $application_folder
	variables at the top of the file with the new name you’ve chosen.

	For the best security, both the system and any application folders
	should be placed above web root so that they are not directly
	accessible via a browser. By default, .htaccess files are included
	in each folder to help prevent direct access, but it is best to
	remove them from public access entirely in case the web server
	configuration changes or doesn’t abide by the .htaccess.

	If you would like to keep your views public it is also possible
	to move the views folder out of your application folder.

	After moving them, open your main index.php file and set the $system_path,
	$application_folder and $view_folder variables, preferably with a
	full path, e.g. ‘/www/MyUser/system’.

	That’s it!

	If you’re new to LavaLust, the User Guide to begin learning how to
	build dynamic PHP applications. Enjoy!


