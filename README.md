<?php

///php.ini Config
// display_errors = On
// memory_limit = 1024M
// max_execution_time = 9999999


//=========================================//
//Github info
$owner = 'mortezaashrafi';
$token = 'ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx';
$repo = 'github-repo-download-for-php';
$ref = 'main';
//Save Location
$location = '';
//=========================================//

//PHP CURL
$curl = curl_init();
curl_setopt_array($curl, array(
  CURLOPT_URL => 'https://api.github.com/repos/' . $owner . '/' . $repo . '/zipball/' . $ref,
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => '',
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 0,
  CURLOPT_FOLLOWLOCATION => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => 'GET',
  CURLOPT_HTTPHEADER => array(
    'Accept: application/vnd.github.v3+json',
    'Authorization: token ' . $token . '',
    'User-Agent: Awesome-Octocat-App'
  ),
));
$response = curl_exec($curl);
curl_close($curl);

//File Put
$file_status = file_put_contents($location . $repo . '.zip', $response);

//Status file download
if (is_numeric($file_status)) {
  echo 'File Size: ' . $file_status . '';
  echo '<br><p style="color: green">Zip file download successful</p><br>';
  unZipFile($location,$repo);
} else {
  echo '<br><p style="color: red">Download failed</p><br>';
}

//UnZip repo
function unZipFile($location,$repo)
{
  $zip = new ZipArchive;
  $res = $zip->open($location . $repo . '.zip');
  if ($res === TRUE) {
    $zip->extractTo($location . $repo);
    $zip->close();
    echo '<br><p style="color: green">UnZip done</p><br>';
  } else {
    echo '<br><p style="color: red">UnZip failed</p><br>';
  }
}
