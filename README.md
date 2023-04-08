CREATE TABLE complaints (
  id INT(11) NOT NULL AUTO_INCREMENT,
  name VARCHAR(255) NOT NULL,
  email VARCHAR(255) NOT NULL,
  complaint TEXT NOT NULL,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (id)
);
<?php
// Connect to the database
$dbhost = 'localhost'; // replace with your database host
$dbname = 'your_database_name'; // replace with your database name
$dbuser = 'your_database_username'; // replace with your database username
$dbpass = 'your_database_password'; // replace with your database password
$conn = new mysqli($dbhost, $dbuser, $dbpass, $dbname);

// Check for errors connecting to the database
if ($conn->connect_error) {
  die('Error connecting to the database: ' . $conn->connect_error);
}

// Sanitize and validate user input
$name = mysqli_real_escape_string($conn, $_POST['name']);
$email = mysqli_real_escape_string($conn, $_POST['email']);
$complaint = mysqli_real_escape_string($conn, $_POST['complaint']);

if (empty($name) || empty($email) || empty($complaint)) {
  die('Error: Please fill in all fields.');
}

if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
  die('Error: Invalid email format.');
}

// Insert the complaint data into the database
$sql = "INSERT INTO complaints (name, email, complaint) VALUES ('$name', '$email', '$complaint')";

if ($conn->query($sql) === TRUE) {
  echo 'Complaint submitted successfully!';
} else {
  echo 'Error submitting complaint: ' . $conn->error;
}

// Close the database connection
$conn->close();
?>
