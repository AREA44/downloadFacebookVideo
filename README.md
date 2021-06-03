# downloadFacebookVideo

This article will guide you **How to download any video on Facebook? (including Private Videos)**, as long as **you can watch** them.

### Step 1.

Install [Custom JavaScript for Websites 2](https://chrome.google.com/webstore/detail/custom-javascript-for-web/ddbjnfjiigjmcpcpkmhogomapikjbjdk/related?hl=vi).

### Step 2.
Open **Custom JavaScript for Websites 2** extension and add this script:
 ```javascript
 function downloadFacebookVideo(){
  var commentQuery = 'comment_id';
  var url = window.location.href;
  if(url.indexOf('?' + commentQuery + '=') != -1)
    return downloadCommentVideo();
  else if(url.indexOf('&' + commentQuery + '=') != -1)
    return downloadCommentVideo();
  return downloadNormalVideo();
}
function downloadCommentVideo(){
  let str = document.documentElement.outerHTML;
  let regex_video_source =  /"edges":\[{(.*?)],"page_info"/gm;
  let regex_video_match_sd = /"playable_url":"(.*?)"/m;
  let regex_video_match_hd = /"playable_url_quality_hd":"(.*?)"/m;
  
  let comment_video_source = null;
  let comment_video_match_sd = null;
  let comment_video_match_hd = null;
  
while ((m = regex_video_source.exec(str)) !== null) {
   comment_video_source = (typeof(m[1]) != "undefined") ? m[1] : null;
   break;
}
if(comment_video_source == null){
  console.log('Can not get this video!');
  return;
}
while ((m = regex_video_match_sd.exec(comment_video_source)) !== null) {
    comment_video_match_sd = (typeof(m[1]) != "undefined") ? m[1] : null;
     break;
}
while ((m = regex_video_match_hd.exec(comment_video_source)) !== null) {
    comment_video_match_hd = (typeof(m[1]) != "undefined") ? m[1] : null;
     break;
}

  if(comment_video_match_sd == null && comment_video_match_hd == null){
  console.log("Can not find any video!");
  return;
  }
  let output = 'Using this link to download: \n';
if(comment_video_match_sd != null){
   let link = comment_video_match_sd.replaceAll("\\","");
   let link_final = link.replaceAll("amp;","");
   output += "SD version: " + link_final + "\n";
}
if(comment_video_match_hd != null){
   let link = comment_video_match_hd.replaceAll("\\","");
   let link_final = link.replaceAll("amp;","");
   output += "HD version: " + link_final + "\n";
}
str = null;
return output;

}
function downloadNormalVideo(){
let m;
let str = document.documentElement.outerHTML;
let regex_sd = /"playable_url":"([^"]*)"/gm;
let regex_hd = /"playable_url_quality_hd":"([^"]*)"/gm;
let regex_audio = /"audio":\[(.*?)\]/sm; // Get audio
let regex_audio_url = /{"url":"(.*?)\",/;  // First audio
// Audio
let video_sd_url = null;
let video_hd_url = null;
let audio_json = null;
let audio_url = null;

// Get SD
while ((m = regex_sd.exec(str)) !== null) {
   video_sd_url = (typeof(m[1]) != "undefined") ? m[1] : null;
    break;
}
// Get HD
while ((m = regex_hd.exec(str)) !== null) {
   video_hd_url = (typeof(m[1]) != "undefined") ? m[1] : null;
      break;
}
/* Start getting Audio */
// Audio Json
if ((m = regex_audio.exec(str)) !== null) {
    audio_json = (typeof(m[1]) != "undefined") ? m[1] : null;
}
// Audio URL
if (audio_json != "null" && (m = regex_audio_url.exec(audio_json)) !== null ) {
    audio_url = (typeof(m[1]) != "undefined") ? m[1] : null;
}
/* End getting Audio */

if(video_hd_url == null && video_sd_url == null && audio_url == null){
  console.log("Can not find any video!");
  return;
}

let output = 'Using this link to download: \n';

// Link SD
if(video_sd_url != null){
   let sd = video_sd_url.replaceAll("\\","");
   output += "SD version: " + sd + "\n";
}
// Link HD
if(video_hd_url != null){
   let hd = video_hd_url.replaceAll("\\","");
   output += "HD version: " + hd + "\n";
}
// Link Audio
if(audio_url != null){
   let audio = audio_url.replaceAll("\\","");
   output += "Audio version: " + audio + "\n";
}
str = null;
return output;
}
 ```
 **Save** and **Refresh** to load script.
 
 ### Step 3.
 Press **F12** and call **downloadFacebookVideo()** function.
  
 ## Refering
 1. [Hướng dẫn tải bất cứ video nào trên FaceBook](https://httzip.com/bai-viet/huong-dan-tai-bat-cu-video-nao-tren-facebook?fbclid=IwAR2tMf1NXtaCnMswm-gtFj4M8F7LmrCCpF7GcOMzSkwnm3mACZxnA4KHH3k).
