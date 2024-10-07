<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Patrol Check - Location 1</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/proj4js/2.6.0/proj4.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mgrs/1.0.0/mgrs.min.js"></script>
    <script>
        // Function to get the current date and time
        function getCurrentDateTime() {
            const now = new Date();
            return now.toLocaleString();
        }

        // Function to get the current location
        function getLocation() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(showPosition);
            } else {
                alert("Geolocation is not supported by this browser.");
            }
        }

        // Function to show the position in MGRS format
        function showPosition(position) {
            const lat = position.coords.latitude;
            const lon = position.coords.longitude;
            const mgrsCoord = mgrs.forward([lon, lat]);
            document.getElementById('location').value = mgrsCoord;

            // Also store latitude and longitude for Google Maps link
            document.getElementById('latitude').value = lat;
            document.getElementById('longitude').value = lon;
        }

        // Function to update the WhatsApp link with form information
        function updateWhatsAppLink() {
            const place = document.getElementById('place').value;
            const name = document.getElementById('name').value;
            const datetime = document.getElementById('datetime').value;
            const location = document.getElementById('location').value;
            const latitude = document.getElementById('latitude').value;
            const longitude = document.getElementById('longitude').value;

            const googleMapsLink = `https://www.google.com/maps/search/?api=1&query=${latitude},${longitude}`;
            const message = `Place: ${place}%0AName: ${name}%0ADTG: ${datetime}%0ALocation: ${location}%0AGoogle Maps: ${googleMapsLink}`;
            const phoneNumber = '+9647845301433';
            const whatsappURL = `https://api.whatsapp.com/send?phone=${phoneNumber}&text=${message}`;

            // Update the WhatsApp link
            document.getElementById('whatsappLink').href = whatsappURL;
        }

        // Function to be called when the form is submitted
        function sendWhatsAppMessage() {
            const name = document.getElementById('name').value;
            if (!name) {
                alert('Please enter your name.');
                return;
            }

            // Update the WhatsApp link with the form information
            updateWhatsAppLink();

            // Open the WhatsApp link in a new tab
            window.open(document.getElementById('whatsappLink').href, '_blank');
        }

        // Set the current date and time on page load
        window.onload = function() {
            document.getElementById('datetime').value = getCurrentDateTime();
            document.getElementById('place').value = "Location 1";
            getLocation();
        };
    </script>
</head>
<body>
    <form onsubmit="return false;">
        <label for="place">Place:</label><br>
        <input type="text" id="place" name="place" readonly><br><br>
        
        <label for="name">Name:</label><br>
        <input type="text" id="name" name="name" required><br><br>
        
        <label for="datetime">Date & Time:</label><br>
        <input type="text" id="datetime" name="datetime" readonly><br><br>
        
        <label for="location">Location (MGRS):</label><br>
        <input type="text" id="location" name="location" readonly><br><br>

        <input type="hidden" id="latitude" name="latitude">
        <input type="hidden" id="longitude" name="longitude">

        <a id="whatsappLink" href="#" onclick="sendWhatsAppMessage()">Check</a>
    </form>
</body>
</html>
