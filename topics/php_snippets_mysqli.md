# Start php with this timezone set
```php
date_default_timezone_set("Asia/Kolkata");
```

# Strip HTML tags off of any `$_POST` before submitting into database.
```php
if (!empty($_POST)) {
	foreach ($_POST as $key => $value) {
		$_POST[$key] = is_array($key) ? $_POST[$key]: strip_tags($_POST[$key], '<a><b><u><i>');
	}
}
```

# PHP connect to DB
```php
$link = mysqli_connect("localhost", "user", "pass","database");
```

# PHP MySQL SELECT
```php
$query = "SELECT * FROM table WHERE column = '".mysqli_real_escape_string($link, $_GET['columnvalue'])."' ORDER BY column DESC LIMIT 1";
$result = mysqli_query($link, $query);
$row = mysqli_fetch_assoc($result);
```

## Check if MySQL returned null results
```php
if (mysqli_num_rows($result)==0) { PERFORM ACTION }
```

## While loop over php query results
```php
while ($row = mysqli_fetch_array($result)) {
		print_r($row);
		echo "<br>";
}
```

# PHP MySQL INSERT
```php
$query = "INSERT INTO table (heading, timestamp) 
  VALUES (
  '".mysqli_real_escape_string($link, $_POST['heading'])."',
  '".date('d-m-Y h:i:sa')."');";

if(mysqli_query($link, $query)) {
    $entry_id = mysqli_insert_id($link); //get id of most recent inserted row
    header("Location: view.php?id=".$entry_id."&successEdit=1"); //redirect
}
```

# PHP MySQL UPDATE
```php
$query1 = "UPDATE table 
SET heading = '".mysqli_real_escape_string($link, $_POST['heading'])."',
body = '".mysqli_real_escape_string($link, $_POST['body'])."'
WHERE table.id = '".mysqli_real_escape_string($link, $_GET['id'])."' LIMIT 1";
```

# PHP MySQL DELETE
```php
$query = "DELETE FROM table WHERE table.id = '". $_GET['id'] ."' LIMIT 1";
```

# regex phone url email
```php
$phone_regex = '([+]?\d[-\s]?\d+)';
$row['phone'] = preg_replace($phone_regex, '<a href="tel:$0" title="$0">$0</a> <span class="text-muted">(Call)</span>', $row['phone']);
echo '<p><b>Phone: </b>'.$row['phone'].'</p>';

$row['body'] = htmlspecialchars($row_get_entry['body'], ENT_QUOTES);
$url_regex = '@(http(s)?)?(://)?(([a-zA-Z])([-\w]+.)+([^\s.]+[^\s])+[^,.\s])@';
$row['body'] = preg_replace($url_regex, '<a href="http$2://$4" target="_blank" title="$0">$0</a>', $row['body']);
echo '<td>' . $row['body'] . '</td>';

$emailregex  = '/([a-zA-Z0-9.-]+@[a-zA-Z0-9.-]+.[a-zA-Z]{2,4})/';
$row['email'] = preg_replace($email_regex, '<a href="mailto:$1">$1</a>', $row['email']);
echo '<p><b>Email: </b>'.$row['email'].'</p>';
```

# Check if array key exists
```php
if (array_key_exists("submit", $_POST)) { }
```

# isset vs empty
[SO](https://stackoverflow.com/a/13045314/2365231), 
[Type comparison useful chart](http://php.net/manual/en/types.comparisons.php)
```php
if (isset($_POST["mail"]) && !empty($_POST["mail"])) {
    echo "Yes, mail is set";    
}
```
So `$_POST` is always set, but its content might be empty.

Since `!empty()` already checks whether the value is set, you can also use this version:
```php
if (!empty($_POST["mail"])) {
    echo "Yes, mail is set";    
}
```

# Images
## Display images from pre-identified directory without dot folders or selected files
```php
//full directory of the files
$dir = getcwd().'/pics/small/'.$_GET['folder_name'];
//get all file names in the directory and remove the first 2 value which are . & ..
$file_to_remove = ['d.jpg', 'c.jpg'];
$files = array_filter(array_slice(scandir($dir), 2), function ($element) use ($file_to_remove) { 
     return ($element != $file_to_remove[0] AND  $element != $file_to_remove[1]); 
} ); 
foreach( $files as $file ){
    echo '<a href="pics/big/'.$_GET['clg_reg'].'/'.$file.'" target="_blank">';
    echo '<img src="pics/small/'.$_GET['clg_reg'].'/'.$file.'">';
    echo '</a>';
}
```

## Random image from a directory
```php
function showImage() {
	//full directory of the files
	$dir = getcwd().'/backgrounds/resized';
	//get all file names in the directory and remove the first 2 value which are . & ..
	$files = array_slice(scandir($dir), 2);
	//count total file names returned, -1 cuz we counting from 0, but count() counts from 1
	$files_length = count($files)-1;
	//get a random number from 0 to $files_length (both inclusive)
	$random_file_index = mt_rand (0,$files_length);
	//echo the path for that file using random_file_index as the index number for the file in the array
	echo '/backgrounds/resized/'.$files[$random_file_index];
}
```

# Pagination
[Best tutorial](http://www.phpfreaks.com/tutorial/basic-pagination),
[Stack Overflow](https://stackoverflow.com/a/11196351/2365231),
[Bootstrap paginator](https://getbootstrap.com/docs/4.1/components/pagination/)

## Get next and previous database entry for pagination
```php
$prevquery= "SELECT * FROM saturnalia_yearbook WHERE id < ".mysqli_real_escape_string($link, $row['id'])." ORDER BY id DESC LIMIT 1"; 
$prevresult = mysqli_query($link, $prevquery);
$prevrow = mysqli_fetch_array($prevresult);
// returns NULL if no row to fetch. http://php.net/manual/en/mysqli-result.fetch-array.php
// isset() on a null value returns false, empty() on a null value returns true

$nextquery= "SELECT * FROM saturnalia_yearbook WHERE id > ".mysqli_real_escape_string($link, $row['id'])." ORDER BY id ASC LIMIT 1"; 
$nextresult = mysqli_query($link, $nextquery);
$nextrow = mysqli_fetch_array($nextresult);

if (isset($prevrow)) { }
if (isset($nextrow)) { }
```

# PHP datetime
MySQL stores dates as `Y-m-d`. [SO](https://stackoverflow.com/a/2215359/2365231)
```html
<input type="date"> <!-- Y-m-d\TH:i -->
```
```php
$date = DateTime::createFromFormat('Y-m-d\TH:i', $_POST['date'])->format('d-m-Y H:i');
date("d-m-Y", strtotime($row['dob']));
date("d-M-Y", strtotime('1st April, last year')); //01-04-2018
date('Y-m-d H:i:s') //MySQL datetime format
```

# Quick printing in HTML
```php
<?=$query?>
```

# E-mailing
```php
if (array_key_exists("submit", $_POST) AND $_POST['submit']=='sendmail') {
	if(!$_POST['email']) {
		$error.= "An email address is required.<br>";
	}
	if(!$_POST['message']) {
		$error.= "A message body is required.<br>";
	}
	if($_POST['email'] && filter_var($_POST['email'], FILTER_VALIDATE_EMAIL) == false) {
		$error.= $_POST['email'].": Not a valid email address.<br>";
	}
	if($error== "") {
		$emailTo = "myownemail@gmail.com";
		$subject = "Email from example.com";
		$body = $_POST['message'];
		$headers = 'From: visitor@example.com' . "\r\n" .
		'Reply-To: ' . $_POST['email'] . "\r\n" .
		'X-Mailer: PHP/' . phpversion();
		if (mail($emailTo, $subject, $body, $headers)) {
			$success = 'mail sent successfully';
		} else {
			$error.= 'Mailing failed';
		}
	}
}
```

# Uploading
Ajax File Receiver located as `includes/ajax-receiver.php`. This uploads the files to the user Id's folder.

```php
session_start();

//allowed file types
$arr_file_types = ['image/png', 'image/gif', 'image/jpg', 'image/jpeg'];
 
if (!(in_array($_FILES['file']['type'], $arr_file_types))) {
    echo "type"; //for javascript to pick up and output "invalid file type"
    return;
}

// create the path if not exists
$path= '../files/'.$_SESSION['id']; //relative to where this php file is located.
if (!file_exists($path)) {
    // recursively create the full path
    // https://stackoverflow.com/a/15012257/2365231
    mkdir($path, 0777, true);
}

//Check if File's already been uploaded
if (file_exists("../files/".$_SESSION['id']."/".($_FILES["file"]["name"]))) {
    echo 'exists';
    return;
// If it's not uploaded , then :
} else {
    //Then Save it that user's user id folder!
    
    move_uploaded_file($_FILES['file']['tmp_name'], $path.'/'. $_FILES['file']['name']);
    
    // echo "Upload: ".$_FILES["file"]["name"]."<br>";
    // echo "Type: ".$_FILES["file"]["type"]."<br>";
    // echo "Size: ".($_FILES["file"]["size"] / 1048576)." MB <br>";
    // echo "Temp file:". $_FILES["file"]["tmp_name"]."<br>";
    echo "Stored in: <a href='files/".$_SESSION['id']."/". rawurlencode($_FILES["file"]["name"])."' target='_blank'>"."files/".$_SESSION['id'] ."/". $_FILES["file"]["name"]."</a>";
    $textarea="typeInTextarea(document.getElementById('body'), '[','](files/".$_SESSION['id']."/".rawurlencode($_FILES['file']['name']).")')";
    echo ' <a href="#" class="badge badge-success" onclick="'.$textarea.'">Add Link</a>';
}
```

Sender:
```html
<form>
    <input type="file" name="image" class="image mb-2" />
    <button type="submit" name="submit" class="submit mb-2">Submit</button>
</form>
```

```javascript
$(function() {
    $('.submit').on('click', function() {
        var file_data = $('.image').prop('files')[0];
        if (file_data != undefined) {
            var form_data = new FormData();
            form_data.append('file', file_data);
            $.ajax({
                type: 'POST',
                url: 'includes/ajax-receiver.php',
                contentType: false,
                processData: false,
                data: form_data,
                success: function(response) {
                    if (response == 'type') {
                        alert('Invalid file type');
                    } else if (response == 'exists') {
                        alert('File already exists');
                    } else {
                        $(".output").append("<p>" + response + "</p>");
                    }
                    $('.image').val('');
                }
            });
        }
        return false;
    });
});
```

# PHP Servers
*Start a server*
```bash
php -S localhost:8000
```

*End a server*
```bash
PHP 5.4.0 Development Server started at Thu Jul 21 10:43:28 2011
Listening on localhost:8000
Document root is /home/me/public_html
Press Ctrl-C to quit.
```
If you don't have that terminal open, you have to find the PID of the process and kill that.

# Composer
Initiate composer in a project or update the dependencies subsequently:
```bash
composer install
composer dump-autoload
```