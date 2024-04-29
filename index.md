---
layout: home
search_exclude: true
---
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Parallax Scrolling</title>
    <style>
        /* Add some basic styling */
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            scroll-behavior: smooth;
        }
        .parallax-container {
            height: 300%; /* Set the height to allow for scrolling */
            width: 100%;
            overflow-x: hidden;
            overflow-y: auto;
            position: relative;
        }
        .parallax-image {
            background-attachment: fixed; /* Fix the background image for parallax effect */
            background-size: cover;
            background-position: center;
            height: calc(100vh - 3px); /* Set height to viewport height minus 3 pixels */
            width: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 0; /* Remove extra margin for smooth transition */
        }
        /* Add spacing between images */
        .spacer {
            height: 30vh; /* Set the height of the spacer */
            display: flex;
            justify-content: center; /* Center the content horizontally */
            align-items: center; /* Center the content vertically */
        }
        /* Style the links in the spacers */
        .spacer a {
            font-size: 96px; /* Set text font size to 4 times bigger (4 * 24) */
            font-weight: bold; /* Make text bold */
            color: blue; /* Set text color */
            text-decoration: none; /* Remove underline */
        }
    </style>
</head>

<body>

<div class="parallax-container">
    <!-- Water Polo section -->
    <div class="parallax-image" style="background-image: url('https://w0.peakpx.com/wallpaper/595/522/HD-wallpaper-grapefruit-lemonade-pink-drink-lemon-lime.jpg')">
    </div>
    <!-- Spacer between Water Polo and Fitness -->
    <div class="spacer">
        <a href="{{site.baseurl}}/score/">Water Polo</a>
    </div>
    <!-- Fitness section -->
    <div class="parallax-image" style="background-image: url('https://www.sleep.org.au/image_cache/sleep/default_main_image_001-380x214.jpeg')">
    </div>
    <!-- Spacer between Fitness and TBFT Simulator -->
    <div class="spacer">
        <a href="{{site.baseurl}}/fitness.html">Fitness</a>
    </div>
    <!-- TBFT Simulator section -->
    <div class="parallax-image" style="background-image: url('https://cdn.vox-cdn.com/thumbor/rugU544JimUJ6WKr9TJEUY2duYk=/0x0:2040x1360/1820x1213/filters:focal(907x191:1233x517):format(webp)/cdn.vox-cdn.com/uploads/chorus_image/image/69435130/tarkov_hero.0.jpg')">
    </div>
    <!-- Spacer after TBFT Simulator -->
    <div class="spacer">
        <a href="{{site.baseurl}}/main_menu">TBFT Simulator</a>
    </div>
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
            "https://www.nme.com/wp-content/uploads/2023/01/Escape-From-Tarkov-Streets-of-Tarkov-1392x884.jpg"
        ]
    ];

    const parallaxImages = document.querySelectorAll('.parallax-image');
    let currentImageIndex = [0, 0, 0];

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

    // Change images every 5 seconds
    setInterval(changeImages, 3000);
</script>

</body>
</html>
