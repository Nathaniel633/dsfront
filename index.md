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
        <a href="{{site.baseurl}}/score/">Water Polo</a>
    </div>
    <!-- New section for sleep -->
    <div class="parallax-image" style="background-image: url('https://www.sleep.org.au/image_cache/sleep/default_main_image_001-380x214.jpeg')">
        <a href="{{site.baseurl}}/fitness.html">Fitness</a>
    </div>
    <!-- TBFT game carousel w/ links -->
    <div class="parallax-image" style="background-image: url('https://cdn.vox-cdn.com/thumbor/rugU544JimUJ6WKr9TJEUY2duYk=/0x0:2040x1360/1820x1213/filters:focal(907x191:1233x517):format(webp)/cdn.vox-cdn.com/uploads/chorus_image/image/69435130/tarkov_hero.0.jpg')">
        <a href="{{site.baseurl}}/main_menu">TBFT Simulator</a>
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
            "https://t3.ftcdn.net/jpg/02/63/47/62/360_F_263476261_hmLxE3xIQHY8KF1PRxGFCMyLBdjKxZnU.jpg",
            "https://wallpapers.com/images/hd/water-polo-yellow-ball-436aaksptls4edif.jpg",
            "https://res.cloudinary.com/usopc-prod/image/upload/v1684930080/TeamUSA%20Assets/Migration/Pages/Johnson_Ashleigh_070222_1440x810_Updated.jpg"

        ],
        [
            "https://insider.fitt.co/wp-content/uploads/2024/02/AssaultFitness_Runner_0611-e1709038770512.jpeg",
            "https://ascendancyfitnessgym.com/img/gallery/gymfloor5.jpg",
            "https://www.pennmedicine.org/-/media/images/miscellaneous/fitness%20and%20sports/fitness_gear_purple.ashx",
            "https://media.istockphoto.com/id/1385881889/photo/modern-gym-interior-with-barbell-dumbbells-exercise-bike-and-ultraviolet-neon-lights.jpg?s=612x612&w=0&k=20&c=GidJT63_7UICAyOewzaK-Fl2GFPBNqR0_4PYYEYugCQ="
        ],
        [
            "https://cdn.vox-cdn.com/thumbor/rugU544JimUJ6WKr9TJEUY2duYk=/0x0:2040x1360/1820x1213/filters:focal(907x191:1233x517):format(webp)/cdn.vox-cdn.com/uploads/chorus_image/image/69435130/tarkov_hero.0.jpg",
            "https://www.esports.net/wp-content/uploads/2023/11/escape-from-tarkov-t-e1700029789713.jpg",
            "https://www.nme.com/wp-content/uploads/2023/01/Escape-From-Tarkov-Streets-of-Tarkov-1392x884.jpg",
        ]
    ];

    const parallaxImages = document.querySelectorAll('.parallax-image');
    let currentImageIndex = [0, 0, 0];
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

</body>
</html>


