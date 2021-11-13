---
title: "Verify your COVID-19 vaccination pass"
date: 2021-11-13T01:26:38+13:00
draft: false
markup: html
tags: [ "interactive", "internet technologies" ]
---

<div id="major">
    <h3 id="title">Show your QR code to the camera!</h3>

    <video id="preview"></video>

    <div id="detailsbox">
        <table id="detailstable">
            <tbody>
              <tr>
                <td><p>Status:</p></td>
                <td><p id="verifystatus">None yet</p></td>
              </tr>
              <tr>
                <td><p>Name:</p></td>
                <td><p id="name">N/A</p></td>
              </tr>
              <tr>
                <td><p>Date of Birth:</p></td>
                <td><p id="dob">N/A</p></td>
              </tr>
            </tbody>
        </table>
    </div>

    <p>Credit to <a href="https://pancake.nz/">pancake.nz</a> for underlying API technology.</p>

</div>

<style>
#major {
  text-align: center;
}
#detailsbox {
  border: black solid 6px;
  border-radius: 12px;
  padding: 20px;
  margin: auto;
  margin-bottom: 20px;
}
#detailstable {
  margin: auto;
}
@media (min-width: 400px) {
    #detailsbox{
        width: 370px;
    }
}
#preview {
  margin: auto;
  margin-top: 20px;
  margin-bottom: 20px;
}
#title {
  text-align: center;
}
</style>

<script src="https://rawgit.com/schmich/instascan-builds/master/instascan.min.js"></script>

<script type="text/javascript"> 

    let scanner = new Instascan.Scanner({
      video: document.getElementById('preview')
    });

    scanner.addListener('scan', function (content) {
      console.log(content);
      fetch("https://api.pancake.nz/verify", {
          body: JSON.stringify({
            "payload": content
          }),
          headers: {
            "Content-Type": "application/json",
          },
          method: "POST"
        })
        .then(response => response.json())
        .then(data => {
          console.log(data)
          if (data.verified == true) {
            setBorderToColour("#34c759")
            document.getElementById("verifystatus").innerHTML = "Verified!";
            document.getElementById("name").innerHTML = data.payload.givenName + " " + data.payload.familyName;
            document.getElementById("dob").innerHTML = data.payload.dob;
          } else {
            setBorderToColour("#ff0000")
            document.getElementById("verifystatus").innerHTML = "Not verified!";
            document.getElementById("name").innerHTML = "Hidden for privacy reasons.";
            document.getElementById("dob").innerHTML = "Hidden for privacy reasons.";
          }
        })
    });

    Instascan.Camera.getCameras().then(function (cameras) {
      if (cameras.length > 0) {
        scanner.start(cameras[0]);
      } else {
        console.error('No cameras found.');
      }
    }).catch(function (e) {
      console.error(e);
    });

    function setBorderToColour(a) {
      document.getElementById("preview").style.border = a + " solid 12px";
      document.getElementById("preview").style.borderRadius = "24px"
    }

</script>