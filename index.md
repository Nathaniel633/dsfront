---
layout: home
search_exclude: true
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Parallax Scrolling</title>
    <style>
        /* Add some basic styling */
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
        }
        .parallax-container {
            height: 200%; /* Set the height to 200% to allow for scrolling */
            width: 100%;
            overflow-x: hidden;
            overflow-y: auto;
            position: relative;
            margin-bottom: 30px; /* Wider gap between "Game" and "Sleep" */
        }
        .parallax-image {
            background-size: cover;
            background-position: center;
            height: calc(70vh - 3px); /* Set the height to 50% of the viewport height minus 3 pixels */
            width: 100%;
            border: 8px ;
            box-sizing: border-box;
            display: flex;
            justify-content: center;
            align-items: center;
            margin-top: calc(10vh + 3px); /* Center the image vertically */
            position: relative;
        }
        .parallax-image:nth-child(1) {
            margin-bottom: 30px; /* Wider gap between the images */
        }
        .parallax-image a {
            font-size: 48px; /* Make the font two times bigger */
            font-weight: bold; /* Make the font bold */
            position: absolute;
            top: -70px; /* Adjust vertical position to place text in the gap */
            left: 50%; /* Center horizontally */
            transform: translateX(-50%); /* Center horizontally */
            color: white; /* Set text color */
            text-decoration: none; /* Remove underline */
            z-index: 1; /* Ensure the title is above the image */
        }
        /* Customize title colors */
        .parallax-image:nth-child(1) a { color: #762DAF; } /* Purple */
        .parallax-image:nth-child(2) a { color: #897FFF; } /* white */
    </style>
</head>
<body>

<div class="parallax-container">
    <!-- Sets up the links to different pages in the site -->
    <div class="parallax-image" style="background-image: url('https://w0.peakpx.com/wallpaper/595/522/HD-wallpaper-grapefruit-lemonade-pink-drink-lemon-lime.jpg')">
        <a href="http://127.0.0.1:4200/student/game">Game</a>
    </div>
    <!-- New section for sleep -->
    <div class="parallax-image" style="background-image: url('https://www.sleep.org.au/image_cache/sleep/default_main_image_001-380x214.jpeg')">
        <a href="http://127.0.0.1:4100/Nighthawk-Pages/fitness.html">Fitness</a>
    </div>
    <div class="parallax-bar bar1"></div>
    <div class="parallax-bar bar2"></div>
    <div class="parallax-bar bar3"></div>
    <div class="parallax-bar bar4"></div>
</div>

<script>
    /* Sets up the different images used when rotating through images, can add new ones into each list, are all organized based on the different heading they appear under */
    const images = [
        [
            "https://w0.peakpx.com/wallpaper/595/522/HD-wallpaper-grapefruit-lemonade-pink-drink-lemon-lime.jpg",
            "https://t3.ftcdn.net/jpg/02/98/48/06/360_F_298480639_9Xz6EzHwA3MoKPVvM7S6IRzRFbqpBe8A.jpg",
            "https://www.southernliving.com/thmb/RxQPgSjPvE_qqjjNPCgJjJeaats=/1500x0/filters:no_upscale():max_bytes(150000):strip_icc()/4ab70ff2-4373-4805-985f-005ee5256743_0-76feabfb1458442d8aaef8e5bd1bcc7e.png",
            "https://cache.desktopnexus.com/thumbseg/2700/2700303-bigthumbnail.jpg",
            "https://t4.ftcdn.net/jpg/06/57/04/13/360_F_657041372_AUVKfXPALQc9u1L2IXJCRtNQG5MJWOR5.jpg",
            "https://t3.ftcdn.net/jpg/05/09/71/00/360_F_509710059_CdNe0lESIhajbQ5g9MBoJVrzmaNnAbUi.jpg",
            "https://media.istockphoto.com/id/949446594/photo/pomegranate-tequila-cocktail-summer-light-alcoholic-drink-cooling-aperitif-on-light-background.webp?b=1&s=170667a&w=0&k=20&c=PcFX8YOfHeVYT5wfIA5DHQx9XVlrzkvIsL94HKQMTpM="

        ],
        [
            "https://img.freepik.com/premium-photo/pink-stationery-pink-background_573856-1067.jpg",
            "https://i.pinimg.com/736x/95/d7/26/95d7264d4ac2564650941330b2b43103.jpg",
            "https://as2.ftcdn.net/v2/jpg/01/73/81/93/1000_F_173819337_Z4fv6wOPFCP9UXT1iON1Ev2i4HvzXmsx.jpg",
            "https://marketplace.canva.com/EAFIpxoov1U/1/0/1600w/canva-pink-modern-organizer-desktop-wallpaper-s7kqOFE3dyA.jpg"
        ]
    ];

    const parallaxImages = document.querySelectorAll('.parallax-image');
    let currentImageIndex = [0, 0];
    // This initializes an array "currentImageIndex" with five elements, all set to 0. This array will keep track of the index of the current image being displayed for each parallax image.

    // Function to change images
    function changeImages() {
        parallaxImages.forEach((image, index) => {
            const imagesIndex = currentImageIndex[index];
            image.style.backgroundImage = `url('${images[index][imagesIndex]}')`;
            currentImageIndex[index] = (imagesIndex + 1) % images[index].length;
        });
    }

    // Initial call to change images
    changeImages();

    // Change images every 3 seconds
    setInterval(changeImages, 3000);
</script>


<!---Heading text-->
<h1 style="text-align: center">Project WRONG</h1>
<h2>Jason Gao, Rayyan Darugar, Grayson Guyot, Nathan Obodovski, Will Bartelt</h2>
<br>

<!--Image work-->
<div class="learning">
    <img src="https://i.postimg.cc/VvyJb6mF/WRONG-learn.png">
    <img src="https://i.postimg.cc/0jpbGsN1/WRONG-test.png">
</div>

<!--Buttons-->

<a href="{{site.baseurl}}/main_menu" class="button">
    <button>
      TBFT Game
    </button>
  </a>
</body>
</html>


