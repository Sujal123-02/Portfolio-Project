# Portfolio-Project

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Sujal Chaudhari's Dynamic Portfolio</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css"
        integrity="sha512-SnH5WK+bZxgPHs44uWIX+LLJAJ9/2PkPKZ5QiAj6Ta86w+fsb2TkcmfRyVX3pBnMFcV7oQPJkl9QevSCWr3W6A=="
        crossorigin="anonymous" referrerpolicy="no-referrer" />
    <style>
        /* --- Styles adapted from the single-page (Green Theme) code --- */
        :root {
            /* Green Theme Color Palette */
            --accent-color: #1db954; /* Spotify Green */
            --accent-color-hover: #1aa34a; /* Darker Green */
            --bg-color-dark: #191924; /* Very Dark Blue/Gray */
            --bg-color-section: #000000; /* Black for sections */
            --bg-color-item: #2a2a34; /* Dark Gray for cards/items */
            --bg-color-item-alt: #333834; /* Alt dark gray */
            --text-color-primary: #e0e0e0; /* Light Gray */
            --text-color-secondary: #b0b0b0; /* Medium Gray */
            --text-color-link: var(--accent-color);
            --text-color-link-hover: #ffffff; /* White link hover */
            --border-color: #444; /* Dark border */
            --glow-color: var(--accent-color);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        html {
            scroll-behavior: smooth;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; /* Kept font */
            background-color: var(--bg-color-dark);
            color: var(--text-color-primary);
            overflow-x: hidden;
            position: relative;
            /* Removed gradient background animation */
        }

        /* --- Page Container --- */
        .page-container {
            position: relative;
            width: 100%;
            min-height: 100vh;
            display: flex; /* Use flex to manage landing vs pages */
            flex-direction: column;
        }

        /* --- Individual Pages --- */
        .page {
            /* position: absolute; */ /* Changed for better flow */
            /* top: 0; */
            /* left: 0; */
            width: 100%;
            min-height: 100vh; /* Ensure page takes full viewport height */
            padding: 0 5%; /* Removed top padding, handled by wrapper */
            opacity: 0;
            visibility: hidden;
            pointer-events: none;
            transition: opacity 0.5s ease, visibility 0.5s ease;
            overflow-y: auto; /* Allow scrolling within page if needed */
            background-color: var(--bg-color-dark);
            display: none; /* Hide pages by default */
            flex-direction: column; /* To push footer down */
            flex-grow: 1; /* Allow page to grow */
        }

        .page-content-wrapper {
            flex-grow: 1; /* Takes up available space */
            padding: 40px 5% 50px 5%; /* Added top padding inside wrapper */
            background-color: var(--bg-color-section); /* Black section background */
            border-radius: 8px; /* Add rounding to content area */
            margin: 80px 0 20px 0; /* Adjust top margin for nav */
            width: 100%; /* Ensure it spans width */
        }

        .page.active {
            opacity: 1;
            visibility: visible;
            pointer-events: auto;
            z-index: 10;
            display: flex; /* Show active page using flex */
             /* position: relative; Re-enable relative if needed for absolute children */
        }

         /* --- Page Specific Transitions (Keep these) --- */
         /* Note: These might need JS to trigger correctly when pages become active */
         #page-about.active { animation: slideInLeft 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94) forwards; }
         @keyframes slideInLeft { from { transform: translateX(-100%); opacity: 0; } to { transform: translateX(0); opacity: 1; } }
         #page-skills.active { animation: fadeInScaleUp 0.7s ease-out forwards; }
         @keyframes fadeInScaleUp { from { transform: scale(0.9); opacity: 0; } to { transform: scale(1); opacity: 1; } }
         #page-projects.active { animation: slideInRight 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94) forwards; }
         @keyframes slideInRight { from { transform: translateX(100%); opacity: 0; } to { transform: translateX(0); opacity: 1; } }
         #page-certifications.active { animation: fadeInBottom 0.7s ease-out forwards; }
         @keyframes fadeInBottom { from { transform: translateY(50px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
         #page-hobbies.active { animation: zoomIn 0.6s cubic-bezier(0.165, 0.84, 0.44, 1) forwards; }
         @keyframes zoomIn { from { transform: scale(0.5); opacity: 0; } to { transform: scale(1); opacity: 1; } }
         /* #page-articles.active { animation: slideInUp 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94) forwards; } */
         /* @keyframes slideInUp { from { transform: translateY(100%); opacity: 0; } to { transform: translateY(0); opacity: 1; } } */
         #page-contact.active { animation: fadeInTop 0.7s ease-out forwards; }
         @keyframes fadeInTop { from { transform: translateY(-50px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }


        /* --- Landing Page Specific Styles --- */
        .landing {
            height: 100vh; /* Full viewport height */
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            padding: 2rem;
            /* position: relative;  Can be removed if not needed for z-index */
            /* z-index: 5; Let active page handle z-index */
            transition: opacity 0.5s ease, visibility 0.5s ease;
            background-color: var(--bg-color-dark); /* Match body */
            color: var(--accent-color);
            width: 100%; /* Takes full width */
        }

        .landing.hidden {
            opacity: 0;
            visibility: hidden;
            pointer-events: none;
            display: none; /* Ensure it doesn't take up space */
            position: absolute; /* Take out of flow when hidden */
        }

        .landing h1 {
            font-size: clamp(2.5rem, 8vw, 4.5rem);
            color: var(--accent-color);
            animation: glow 1.5s ease-in-out infinite alternate;
            margin-bottom: 1rem;
            background: none;
            -webkit-background-clip: unset;
            background-clip: unset;
            -webkit-text-fill-color: unset;
        }
        @keyframes glow {
            from { text-shadow: 0 0 5px var(--glow-color), 0 0 10px var(--glow-color); }
            to { text-shadow: 0 0 20px var(--glow-color), 0 0 35px var(--glow-color); }
        }

        .landing p {
            font-size: clamp(1.1rem, 4vw, 1.5rem);
            margin-bottom: 2.5rem;
            color: var(--text-color-secondary);
            max-width: 600px; /* Limit width of subtitle */
        }

        /* --- Button Styles (Green Theme) --- */
        .enter-btn,
        .contact-form button,
        /* .article-card a.read-more-btn, */ /* Assuming articles page structure */
        #page-certifications .skill-item a {
            padding: 14px 28px;
            background-color: var(--accent-color);
            color: #000 !important;
            border: none;
            border-radius: 8px;
            font-size: 1.1rem;
            font-weight: bold;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.3s ease, box-shadow 0.3s ease;
            text-decoration: none;
            display: inline-block;
            box-shadow: none;
            background-image: none;
        }
        /* .article-card a.read-more-btn, */ /* Assuming articles page structure */
        #page-certifications .skill-item a {
            padding: 10px 20px;
            font-size: 0.9rem;
        }

        .enter-btn:hover,
        .contact-form button:hover,
        /* .article-card a.read-more-btn:hover, */ /* Assuming articles page structure */
        #page-certifications .skill-item a:hover {
            background-color: var(--accent-color-hover);
            transform: scale(1.05);
            box-shadow: 0 0 15px var(--glow-color);
        }

        /* --- General Content Styling --- */
        h2 { /* Section headings */
            font-size: clamp(2rem, 5vw, 2.5rem);
            color: var(--accent-color);
            text-align: center;
            margin-bottom: 3rem;
            /* Removed padding-top, handled by .page-content-wrapper */
            animation: glowFade 2s infinite alternate;
            background: none;
            -webkit-background-clip: unset;
            background-clip: unset;
            -webkit-text-fill-color: unset;
            text-shadow: none;
        }
        @keyframes glowFade {
             from { text-shadow: 0 0 4px var(--glow-color); opacity: 0.9; }
             to { text-shadow: 0 0 12px var(--glow-color); opacity: 1; }
        }


        /* --- Page Navigation --- */
        .page-nav {
            text-align: center;
            margin-bottom: 3rem;
            background-color: #333;
            padding: 10px 0;
            border-radius: 5px;
            position: absolute; /* Change to absolute relative to page */
            top: 0; /* Position at the top of the page div */
            left: 5%; /* Match page padding */
            right: 5%; /* Match page padding */
            width: 90%; /* Take width considering padding */
            z-index: 50;
            /* Make sticky within the page */
            position: sticky;
            top: 10px; /* Adjust sticky position */
        }

        .page-nav a {
            color: var(--text-color-secondary);
            text-decoration: none;
            font-size: 0.9rem;
            margin: 0 10px;
            padding: 8px 15px;
            position: relative;
            transition: color 0.3s ease, background-color 0.3s ease;
            border-radius: 5px;
        }

        .page-nav a:hover {
            color: var(--text-color-link-hover);
            background-color: rgba(29, 185, 84, 0.2);
        }

        .page-nav a:not(:last-child)::after {
             content: '';
             display: none; /* Separator removed */
        }

        .page-nav a.active-nav-link {
            color: #ffffff;
            font-weight: bold;
            background-color: var(--accent-color);
        }

        /* Container for grid layouts */
        .container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 30px;
            justify-content: center;
            margin-bottom: 40px;
            max-width: 1200px;
            margin-left: auto;
            margin-right: auto;
        }

        /* Project Card Styles */
        .project
        /* .article-card */ { /* Assuming article structure */
            background-color: var(--bg-color-item-alt);
            padding: 25px;
            border-radius: 10px;
            text-align: center;
            transition: transform 0.3s ease, box-shadow 0.3s ease, border 0.3s ease;
            border: 2px solid transparent;
            overflow: hidden;
            display: flex;
            flex-direction: column;
        }

        .project:hover,
        /* .article-card:hover */ { /* Assuming article structure */
            transform: translateY(-5px) scale(1.02);
            box-shadow: 0 0 15px var(--glow-color);
            border-color: var(--accent-color);
             border-image: none;
        }

        .project img {
            width: 100%;
            height: 200px;
            object-fit: cover;
            border-radius: 8px;
            margin-bottom: 15px;
            transition: transform 0.3s ease;
            background-color: #2a2a34; /* Background color for image area */
        }

        .project:hover img {
            transform: scale(1.05);
        }

        .project h3,
        /* .article-card h3 */ { /* Assuming article structure */
            margin-bottom: 10px;
            color: var(--text-color-primary);
            font-size: 1.4rem;
        }

        .project p,
        /* .article-card p */ { /* Assuming article structure */
            color: var(--text-color-secondary);
            font-size: 0.95rem;
            line-height: 1.6;
            flex-grow: 1; /* Allow text to take space */
            margin-bottom: 1.5rem; /* Space before potential buttons */
            text-align: left; /* Align paragraph text left */
            white-space: pre-line; /* Respect line breaks in the description */
        }

        /* Skills Grid Styles */
        .skills-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 25px;
            margin-bottom: 40px;
            max-width: 1000px;
            margin-left: auto;
            margin-right: auto;
        }

        .skill-item {
            background-color: var(--bg-color-item);
            padding: 25px 20px;
            border-radius: 10px;
            text-align: center;
            transition: transform 0.3s ease, box-shadow 0.3s ease, border-color 0.3s ease;
            border: 2px solid transparent;
            display: flex; /* Use flex for vertical alignment if needed */
            flex-direction: column; /* Stack content vertically */
        }

        .skill-item:hover {
            transform: translateY(-5px) scale(1.02);
            box-shadow: 0 0 12px var(--glow-color);
            border-color: var(--accent-color);
        }

        .skill-item h4 {
            margin-bottom: 15px;
            color: var(--text-color-primary);
            font-size: 1.2rem;
        }

        .skill-item p {
            color: var(--text-color-secondary);
            font-size: 0.9rem;
            line-height: 1.7;
            flex-grow: 1; /* Allow paragraphs to grow */
            margin-bottom: 15px; /* Space below text/tags */
        }

        .skill-item p span {
             display: inline-block;
             margin: 4px 6px;
             padding: 4px 10px;
             background-color: #3e4444;
             border-radius: 5px;
             font-size: 0.85rem;
             color: var(--text-color-primary);
             white-space: nowrap;
        }

        /* Hobbies Grid Styles */
        .hobbies-list {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 25px;
            margin-bottom: 40px;
            max-width: 1000px;
            margin-left: auto;
            margin-right: auto;
        }

        .hobby-item {
            background-color: var(--bg-color-item);
            padding: 25px 20px;
            border-radius: 10px;
            text-align: center;
            transition: transform 0.3s ease, box-shadow 0.3s ease, border-color 0.3s ease;
            border: 2px solid transparent;
        }

        .hobby-item:hover {
            transform: translateY(-5px) scale(1.02);
            box-shadow: 0 0 12px var(--glow-color);
            border-color: var(--accent-color);
        }

        .hobby-item h4 {
            margin-bottom: 10px;
            color: var(--text-color-primary);
            font-size: 1.1rem;
        }

        .hobby-item p {
            color: var(--text-color-secondary);
            font-size: 0.85rem;
        }

         .hobby-item .hobby-emoji {
             font-size: 2.5rem;
             display: block;
             margin-bottom: 15px;
             transition: transform 0.3s ease;
         }
         .hobby-item:hover .hobby-emoji {
             transform: scale(1.2) rotate(10deg);
         }

        /* Certification Styles */
        #page-certifications .skill-item img {
            max-width: 80%;
            height: auto;
            border-radius: 5px;
            margin: 15px auto;
            display: block;
            border: 1px solid #444;
            background-color: #333; /* Background for image area */
        }
         /* Ensure certification links are spaced */
         #page-certifications .skill-item a {
             margin-top: 15px; /* Add space above the button */
         }


        /* About Page Specific Styles */
        #page-about .about-content {
            max-width: 900px;
            margin: 0 auto;
            line-height: 1.8;
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            gap: 30px;
        }

        #page-about .profile-photo-container {
            flex: 0 0 250px;
            text-align: center;
        }

        #page-about .profile-photo {
            width: 250px;
            height: 250px;
            border-radius: 50%;
            object-fit: cover;
            border: 4px solid var(--accent-color);
            box-shadow: 0 0 20px rgba(29, 185, 84, 0.3);
            display: inline-block;
            background-color: var(--bg-color-item); /* Background color for image */
        }

        #page-about .about-text {
            flex: 1;
            min-width: 300px;
            text-align: left;
        }

        #page-about .about-text p {
            font-size: 1.05rem;
            color: var(--text-color-primary);
            margin-bottom: 1.5rem;
        }
         #page-about .about-text p:last-child {
             margin-bottom: 0;
         }

        /* Contact Page Specific Styles */
        #page-contact .contact-container {
            display: flex;
            flex-wrap: wrap;
            gap: 40px;
            justify-content: center;
            align-items: flex-start; /* Align items to top */
            max-width: 1000px;
            margin: 0 auto;
        }

        #page-contact .contact-details,
        #page-contact .contact-form-section {
            flex: 1;
            min-width: 300px;
            max-width: 450px;
            background-color: var(--bg-color-item);
            padding: 30px;
            border-radius: 10px;
        }
         #page-contact .contact-details h3,
         #page-contact .contact-form-section h3 {
             color: var(--text-color-primary);
             margin-bottom: 1.5rem;
             text-align: center;
             font-size: 1.5rem;
         }

        #page-contact .contact-details ul {
            list-style: none;
            padding: 0;
        }

        #page-contact .contact-details li {
            margin-bottom: 15px;
            font-size: 1.1rem;
            color: var(--text-color-secondary);
            display: flex; /* Align icon and text */
            align-items: center; /* Vertically center icon and text */
        }
         #page-contact .contact-details li i {
             margin-right: 10px;
             color: var(--accent-color);
             width: 20px;
             text-align: center;
             flex-shrink: 0; /* Prevent icon from shrinking */
         }

        #page-contact .contact-details a {
            color: var(--text-color-link);
            text-decoration: none;
            transition: color 0.3s ease, text-decoration 0.3s ease;
            word-break: break-all; /* Break long links */
        }

        #page-contact .contact-details a:hover {
            color: var(--text-color-link-hover);
            text-decoration: underline;
        }

        .contact-form {
            width: 100%;
        }

        .contact-form label { /* Add labels for accessibility */
            display: block;
            margin-bottom: 5px;
            color: var(--text-color-secondary);
            font-size: 0.9rem;
            text-align: left;
        }

        .contact-form input,
        .contact-form textarea {
            width: 100%;
            padding: 12px;
            margin-bottom: 15px;
            border: 1px solid #444;
            border-radius: 5px;
            background-color: #333;
            color: var(--text-color-primary);
            font-size: 1rem;
             transition: border-color 0.3s ease, box-shadow 0.3s ease;
        }

        .contact-form input:focus,
        .contact-form textarea:focus {
            border-color: var(--accent-color);
            outline: none;
            box-shadow: 0 0 8px rgba(29, 185, 84, 0.3);
        }

        .contact-form textarea {
            min-height: 120px; /* Give textarea more height */
            resize: vertical; /* Allow vertical resize */
        }

        .contact-form button {
            width: 100%;
            margin-top: 10px;
        }

        /* Footer */
        footer {
            text-align: center;
            padding: 25px 20px;
            background-color: #1f1f1f; /* Slightly different background */
            color: #888;
            font-size: 0.9rem;
            border-top: 1px solid #333;
            width: 100%; /* Span full width */
            margin-top: auto; /* Pushes footer to bottom of flex container */
            /* position: relative; Not needed */
            /* z-index: 1; Not needed */
        }

        .footer-content {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 15px;
            max-width: 1200px; /* Optional: Limit footer width */
            margin: 0 auto; /* Center content */
        }

        .social-icons a {
            color: var(--text-color-secondary);
            font-size: 1.5rem;
            margin: 0 12px;
            transition: color 0.3s ease, transform 0.3s ease;
        }

        .social-icons a:hover {
            color: var(--accent-color);
            transform: scale(1.2);
        }

        /* Visitor Counter Style in Footer */
        .visitor-counter-section {
            margin-top: 15px;
        }
        .visitor-counter-section p {
             color: var(--text-color-secondary);
             margin-bottom: 5px;
             font-size: 0.9rem;
        }
        .visitor-count-display { /* Class used for JS targeting */
            font-size: 1rem;
            font-weight: bold;
            color: #000; /* Black text */
            background: var(--accent-color); /* Green background */
            padding: 5px 15px;
            border-radius: 5px;
            display: inline-block;
            min-width: 30px; /* Ensure space for number */
            text-align: center;
        }


        /* Responsive Adjustments */
        @media (max-width: 768px) {
             .page { padding: 0 5%; } /* Adjust page padding if needed */
             .page-content-wrapper { margin-top: 70px; } /* Adjust margin for potentially wrapped nav */
             h2 { font-size: clamp(1.8rem, 6vw, 2.2rem); } /* Slightly smaller H2 */
             .page-nav { margin-bottom: 2rem; width: 90%; left: 5%; right: 5%; top: 5px; }
             .page-nav a { font-size: 0.8rem; margin: 0 4px; padding: 6px 10px; }
             /* .page-nav a:not(:last-child)::after { right: -6px; } Removed separator */
             .container, .skills-grid, .hobbies-list { grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 20px; }
             .project img { height: 180px; }
             #page-about .about-content { flex-direction: column; text-align: center; }
             #page-about .profile-photo-container { flex-basis: auto; margin-bottom: 20px; }
             #page-about .profile-photo { width: 200px; height: 200px; } /* Smaller photo */
             #page-about .about-text { text-align: center; }
             #page-contact .contact-container { flex-direction: column; align-items: center; }
             #page-contact .contact-details, #page-contact .contact-form-section { max-width: 90%; width:100%; padding: 20px; }
             .footer-content { gap: 10px; }
             .social-icons a { font-size: 1.3rem; margin: 0 8px; }
             .landing h1 { font-size: clamp(2.2rem, 7vw, 4rem); }
             .landing p { font-size: clamp(1rem, 3.5vw, 1.3rem); }
        }

        @media (max-width: 480px) {
             .page-nav { padding: 8px 0; }
             .page-nav a { margin: 2px 3px; font-size: 0.75rem; padding: 5px 8px; }
             /* .page-nav a:not(:last-child)::after { right: -5px; } Removed separator */
              .container, .skills-grid, .hobbies-list { grid-template-columns: 1fr; } /* Force single column */
              #page-about .about-text { text-align: left; } /* Keep text left aligned on mobile */
              #page-about .profile-photo { width: 150px; height: 150px; } /* Even smaller photo */
              #page-contact .contact-details li { font-size: 1rem; } /* Slightly smaller contact font */
              .landing h1 { font-size: clamp(2rem, 7vw, 3.5rem); }
              .landing p { font-size: clamp(0.9rem, 3.5vw, 1.2rem); }
              .enter-btn { padding: 12px 24px; font-size: 1rem; }
        }

        /* Basic JS Functionality Simulation */
        /* Add .active class to #page-about for initial view if landing is hidden */
       /* #page-about, #page-skills, #page-projects, #page-certifications, #page-hobbies, #page-contact {
            display: none;
        } */
        /* Example: To show 'About' page by default if landing is hidden */
        /* .landing.hidden + #page-about {
            display: flex;
            opacity: 1;
            visibility: visible;
            pointer-events: auto;
        } */

    </style>
</head>

<body>

    <div class="page-container">

        <section class="landing" id="landing-page">
            <h1>Welcome to My </h1>
            <h1>    Portfolio</h1> <p>Hi, I'm Sujal Chaudhari — Web Developer | AIML | Creator | Gamer. Let’s dive into my work!</p>
            <a href="#about" class="enter-btn" data-page="page-about">Explore My Work</a>
        </section>

        <div id="page-about" class="page">
            <nav class="page-nav">
                <a href="#about" data-page="page-about" class="active-nav-link">About</a>
                <a href="#skills" data-page="page-skills">Skills</a>
                <a href="#projects" data-page="page-projects">Projects</a>
                <a href="#certifications" data-page="page-certifications">Certifications</a>
                <a href="#hobbies" data-page="page-hobbies">Hobbies</a>
                <a href="#contact" data-page="page-contact">Contact</a>
            </nav>
            <div class="page-content-wrapper">
                <h2>About Me</h2>
                <div class="about-content">
                    <div class="profile-photo-container">
                        <img src="Sujal.jpg" alt="Sujal Chaudhari Profile Photo" class="profile-photo"
                             onerror="this.onerror=null; this.src='https://placehold.co/250x250/2a2a34/b0b0b0?text=Image+Not+Found';">
                    </div>
                    <div class="about-text">
                        <p>Hi everyone, I’m Sujal, a creative soul driven by storytelling and design. I am currently pursuing my degree in Computer Science at MIT Academy of Engineering. With a deep passion for technology and problem-solving, I thrive in building efficient, scalable, and user-friendly solutions.</p>
                        <p>My approach to work combines creativity, efficiency, and problem-solving, allowing me to craft meaningful and impactful projects. I thrive on challenges and am always eager to learn and grow. Over the years, I have worked on diverse projects that have strengthened my skills in web development and beyond.</p>
                        <p>Now talking about my Schooling life, I have completed my high school and junior college from Jajoo English Medium School in my hometown Yavatmal, Maharashtra. I was mainly an average guy who tries to do better in academics and extra-curriculars as well.</p>
                        <h2>Achievements:</h2>
                        <p>My some of the early Achievements are:
                            1.SSC: Got 90%.
                            2.HSC:Got 85%.
                            3.CET:Got 95.26 %ile.
                            4.JEE:86 %ile.
                            5.Qualified For JEE Adv.
                            6.Won prises on chess and batbinton school level Tournament.
                        </p>
                    </div>
                </div>
            </div>
            <footer>
                <div class="footer-content">
                    <div class="social-icons">
                        <a href="mailto:sujalchaudhari0211@gmail.com" aria-label="Email"><i class="fas fa-envelope"></i></a>
                        <a href="https://www.linkedin.com/in/sujalchaudhari0211" target="_blank" aria-label="LinkedIn"><i class="fab fa-linkedin"></i></a>
                        <a href="https://github.com/sujalchaudhari0211" target="_blank" aria-label="GitHub"><i class="fab fa-github"></i></a>
                    </div>
                    <div class="visitor-counter-section">
                        <p>Website Visitors:</p>
                        <span class="visitor-count-display">0</span> </div>
                    <p>&copy; 2025 Sujal Chaudhari. Crafted with Code & Passion.</p>
                </div>
            </footer>
        </div>

        <div id="page-skills" class="page">
             <nav class="page-nav">
                <a href="#about" data-page="page-about">About</a>
                <a href="#skills" data-page="page-skills" class="active-nav-link">Skills</a>
                <a href="#projects" data-page="page-projects">Projects</a>
                <a href="#certifications" data-page="page-certifications">Certifications</a>
                <a href="#hobbies" data-page="page-hobbies">Hobbies</a>
                <a href="#contact" data-page="page-contact">Contact</a>
            </nav>
            <div class="page-content-wrapper">
                <h2>Core Skills</h2>
                <div class="skills-grid">
                     <div class="skill-item"><h4>Programming</h4><p><span>Python</span><span>Java</span><span>C++</span><span>JS</span></p></div>
                     <div class="skill-item"><h4>Web Dev</h4><p><span>HTML</span><span>CSS</span><span>JavaScript</span><span>React</span><span>Node.js</span></p></div>
                     <div class="skill-item"><h4>Tools</h4><p><span>Git</span><span>VS Code</span><span>Photoshop</span></p></div>
                     <div class="skill-item"><h4>Soft Skills</h4><p><span>Teamwork</span><span>Leadership</span></p></div>
                </div>
            </div>
            <footer>
                <div class="footer-content">
                     <div class="social-icons">
                        <a href="mailto:sujalchaudhari0211@gmail.com" aria-label="Email"><i class="fas fa-envelope"></i></a>
                        <a href="https://www.linkedin.com/in/sujalchaudhari0211" target="_blank" aria-label="LinkedIn"><i class="fab fa-linkedin"></i></a>
                        <a href="https://github.com/sujalchaudhari0211" target="_blank" aria-label="GitHub"><i class="fab fa-github"></i></a>
                     </div>
                     <div class="visitor-counter-section">
                        <p>Website Visitors:</p>
                        <span class="visitor-count-display">0</span>
                     </div>
                    <p>&copy; 2025 Sujal Chaudhari. Crafted with Code & Passion.</p>
                </div>
            </footer>
        </div>

        <div id="page-projects" class="page">
             <nav class="page-nav">
                <a href="#about" data-page="page-about">About</a>
                <a href="#skills" data-page="page-skills">Skills</a>
                <a href="#projects" data-page="page-projects" class="active-nav-link">Projects</a>
                <a href="#certifications" data-page="page-certifications">Certifications</a>
                <a href="#hobbies" data-page="page-hobbies">Hobbies</a>
                <a href="#contact" data-page="page-contact">Contact</a>
            </nav>
            <div class="page-content-wrapper">
                <h2>My Projects</h2>
                <div class="container">
                    <div class="project">
                        <img src="https://placehold.co/600x400/1db954/191924?text=Movie+Recommender+UI" alt="Movie Recommender System"
                             onerror="this.onerror=null; this.src='https://placehold.co/600x400/2a2a34/b0b0b0?text=Image+Not+Found';">
                        <h3>🎬Movie Recommender System</h3>
                         <p>A system that suggests movies based on viewing history using collaborative filtering.

Built with: Python, Pandas, Scikit-learn, Streamlit

Trained on: MovieLens dataset
- I created this as a project in my 10th grade.</p>
                    </div>
                    <div class="project">
                        <img src="https://placehold.co/600x400/1db954/191924?text=Virtual+Chem+Lab" alt="SmartLab Virtual Chemistry Lab"
                             onerror="this.onerror=null; this.src='https://placehold.co/600x400/2a2a34/b0b0b0?text=Image+Not+Found';">
                        <h3>🧪SmartLab: Virtual Chemistry Lab Simulator</h3>
                         <p>A simulation app where students can perform chemistry experiments safely in 3D.

Built with: Unity or WebGL + JavaScript

Features: Beaker mixing, reaction predictions, safety warnings</p>
                    </div>
                    <div class="project">
                         <img src="https://placehold.co/600x400/1db954/191924?text=LED+Plant+Growth" alt="Plant Growth Under LED Lights"
                             onerror="this.onerror=null; this.src='https://placehold.co/600x400/2a2a34/b0b0b0?text=Image+Not+Found';">
                        <h3>🌱Plant Growth Under Colored LED Lights</h3>
                         <p>Tested how different light wavelengths affect plant growth.

Setup: Same plant species, 4 different light colors, controlled watering

Results tracked with growth charts and photos over 30 days</p>
                    </div>
                     </div>
            </div>
            <footer>
                <div class="footer-content">
                     <div class="social-icons">
                         <a href="mailto:sujalchaudhari0211@gmail.com" aria-label="Email"><i class="fas fa-envelope"></i></a>
                         <a href="https://www.linkedin.com/in/sujalchaudhari0211" target="_blank" aria-label="LinkedIn"><i class="fab fa-linkedin"></i></a>
                         <a href="https://github.com/sujalchaudhari0211" target="_blank" aria-label="GitHub"><i class="fab fa-github"></i></a>
                     </div>
                     <div class="visitor-counter-section">
                         <p>Website Visitors:</p>
                         <span class="visitor-count-display">0</span>
                     </div>
                    <p>&copy; 2025 Sujal Chaudhari. Crafted with Code & Passion.</p>
                </div>
            </footer>
        </div>

        <div id="page-certifications" class="page">
             <nav class="page-nav">
                <a href="#about" data-page="page-about">About</a>
                <a href="#skills" data-page="page-skills">Skills</a>
                <a href="#projects" data-page="page-projects">Projects</a>
                <a href="#certifications" data-page="page-certifications" class="active-nav-link">Certifications</a>
                <a href="#hobbies" data-page="page-hobbies">Hobbies</a>
                <a href="#contact" data-page="page-contact">Contact</a>
            </nav>
             <div class="page-content-wrapper">
                 <h2>Certifications</h2>
                 <div class="skills-grid"> <div class="skill-item"> <h4>Python Essentials 1</h4>
                         <img src="P1.png" alt="Python Certificate 1"
                              onerror="this.onerror=null; this.src='https://placehold.co/300x150/2a2a34/b0b0b0?text=Cert+Not+Found';" />
                         <p>MIT Academy of Engineering, Pune</p>
                         <p><strong>Date:</strong> April 8, 2025</p>
                         <a href="PE1.pdf" target="_blank">View Certificate</a>
                     </div>
                     <div class="skill-item">
                         <h4>Python Essentials 2</h4>
                         <img src="P2.png" alt="Python Certificate 2"
                              onerror="this.onerror=null; this.src='https://placehold.co/300x150/2a2a34/b0b0b0?text=Cert+Not+Found';" />
                         <p>Cisco Networking Academy</p>
                         <p><strong>Date:</strong> March 27, 2025</p>
                         <a href="PE2.pdf" target="_blank">View Certificate</a>
                     </div>
                     <div class="skill-item">
                         <h4>Python (Basic)</h4>
                         <img src="P3.png" alt="Python Certificate 3"
                              onerror="this.onerror=null; this.src='https://placehold.co/300x150/2a2a34/b0b0b0?text=Cert+Not+Found';" />
                         <p>HackerRank</p>
                         <p><strong>Date:</strong> April 3, 2025</p>
                         <a href="PyB.pdf" target="_blank">View Certificate</a>
                     </div>
                     </div>
             </div>
             <footer>
                 <div class="footer-content">
                     <div class="social-icons">
                         <a href="mailto:sujalchaudhari0211@gmail.com" aria-label="Email"><i class="fas fa-envelope"></i></a>
                         <a href="https://www.linkedin.com/in/sujalchaudhari0211" target="_blank" aria-label="LinkedIn"><i class="fab fa-linkedin"></i></a>
                         <a href="https://github.com/sujalchaudhari0211" target="_blank" aria-label="GitHub"><i class="fab fa-github"></i></a>
                     </div>
                     <div class="visitor-counter-section">
                         <p>Website Visitors:</p>
                         <span class="visitor-count-display">0</span>
                     </div>
                     <p>&copy; 2025 Sujal Chaudhari. Crafted with Code & Passion.</p>
                 </div>
             </footer>
         </div>

         <div id="page-hobbies" class="page">
              <nav class="page-nav">
                 <a href="#about" data-page="page-about">About</a>
                 <a href="#skills" data-page="page-skills">Skills</a>
                 <a href="#projects" data-page="page-projects">Projects</a>
                 <a href="#certifications" data-page="page-certifications">Certifications</a>
                 <a href="#hobbies" data-page="page-hobbies" class="active-nav-link">Hobbies</a>
                 <a href="#contact" data-page="page-contact">Contact</a>
             </nav>
             <div class="page-content-wrapper">
                 <h2>Hobbies & Interests</h2>
                 <div class="hobbies-list">
                     <div class="hobby-item">
                         <span class="hobby-emoji">🎮</span>
                         <h4>Gaming</h4>
                         <p>Exploring virtual worlds and competitive gaming.</p>
                     </div>
                     <div class="hobby-item">
                         <span class="hobby-emoji">💻</span>
                         <h4>Coding</h4>
                         <p>Building side projects and learning new tech.</p>
                     </div>
                     <div class="hobby-item">
                         <span class="hobby-emoji">🎬</span>
                         <h4>Movies & Series</h4>
                         <p>Enjoying captivating stories on screen.</p>
                     </div>
                     <div class="hobby-item">
                         <span class="hobby-emoji">🎵</span>
                         <h4>Music</h4>
                         <p>Listening to various genres, discovering new artists.</p>
                     </div>
                     </div>
             </div>
             <footer>
                 <div class="footer-content">
                     <div class="social-icons">
                         <a href="mailto:sujalchaudhari0211@gmail.com" aria-label="Email"><i class="fas fa-envelope"></i></a>
                         <a href="https://www.linkedin.com/in/sujalchaudhari0211" target="_blank" aria-label="LinkedIn"><i class="fab fa-linkedin"></i></a>
                         <a href="https://github.com/sujalchaudhari0211" target="_blank" aria-label="GitHub"><i class="fab fa-github"></i></a>
                     </div>
                     <div class="visitor-counter-section">
                         <p>Website Visitors:</p>
                         <span class="visitor-count-display">0</span>
                     </div>
                     <p>&copy; 2025 Sujal Chaudhari. Crafted with Code & Passion.</p>
                 </div>
             </footer>
         </div>

         <div id="page-contact" class="page">
              <nav class="page-nav">
                 <a href="#about" data-page="page-about">About</a>
                 <a href="#skills" data-page="page-skills">Skills</a>
                 <a href="#projects" data-page="page-projects">Projects</a>
                 <a href="#certifications" data-page="page-certifications">Certifications</a>
                 <a href="#hobbies" data-page="page-hobbies">Hobbies</a>
                 <a href="#contact" data-page="page-contact" class="active-nav-link">Contact</a>
             </nav>
             <div class="page-content-wrapper">
                 <h2>Get In Touch</h2>
                 <div class="contact-container">
                     <div class="contact-details">
                         <h3>Contact Information</h3>
                         <ul>
                            <li><i class="fas fa-envelope"></i> <a href="mailto:sujalchaudhari0211@gmail.com">sujalchaudhari0211@gmail.com</a></li>
                            <li><i class="fab fa-linkedin"></i> <a href="https://www.linkedin.com/in/sujalchaudhari0211" target="_blank">LinkedIn Profile</a></li>
                            <li><i class="fab fa-github"></i> <a href="https://github.com/sujalchaudhari0211" target="_blank">GitHub Profile</a></li>
                            <li><i class="fas fa-map-marker-alt"></i> Pimpri-Chinchwad, Maharashtra, India</li> </ul>
                     </div>
                     <div class="contact-form-section">
                        <h3>Send Me a Message</h3>
                        <form class="contact-form" action="#" method="POST"> <label for="name">Name:</label>
                            <input type="text" id="name" name="name" required>

                            <label for="email">Email:</label>
                            <input type="email" id="email" name="email" required>

                            <label for="message">Message:</label>
                            <textarea id="message" name="message" required></textarea>

                            <button type="submit" class="enter-btn">Send Message</button>
                        </form>
                     </div>
                 </div>
             </div>
             <footer>
                 <div class="footer-content">
                     <div class="social-icons">
                         <a href="mailto:sujalchaudhari0211@gmail.com" aria-label="Email"><i class="fas fa-envelope"></i></a>
                         <a href="https://www.linkedin.com/in/sujalchaudhari0211" target="_blank" aria-label="LinkedIn"><i class="fab fa-linkedin"></i></a>
                         <a href="https://github.com/sujalchaudhari0211" target="_blank" aria-label="GitHub"><i class="fab fa-github"></i></a>
                     </div>
                     <div class="visitor-counter-section">
                         <p>Website Visitors:</p>
                         <span class="visitor-count-display">0</span>
                     </div>
                     <p>&copy; 2025 Sujal Chaudhari. Crafted with Code & Passion.</p>
                 </div>
             </footer>
         </div>

    </div> <script>
        document.addEventListener('DOMContentLoaded', () => {
            const navLinks = document.querySelectorAll('.page-nav a, .enter-btn');
            const pages = document.querySelectorAll('.page');
            const landingPage = document.getElementById('landing-page');
            const firstPageId = 'page-about'; // Default page after landing

            function showPage(pageId) {
                // Hide landing page if showing another page
                if (pageId && landingPage) {
                    landingPage.classList.add('hidden');
                }

                let foundPage = false;
                pages.forEach(page => {
                    if (page.id === pageId) {
                        page.classList.add('active');
                        foundPage = true;
                    } else {
                        page.classList.remove('active');
                    }
                });

                 // If no specific page found (e.g., initial load with hidden landing), show default
                 if (!foundPage && landingPage && landingPage.classList.contains('hidden')) {
                     const defaultPage = document.getElementById(firstPageId);
                     if (defaultPage) {
                         defaultPage.classList.add('active');
                         // Update nav link for default page
                         updateNavLinks(firstPageId);
                     }
                 } else if (!pageId && !landingPage.classList.contains('hidden')) {
                    // If navigating 'back' to landing (e.g. future feature), show landing
                     landingPage.classList.remove('hidden');
                     pages.forEach(page => page.classList.remove('active'));
                 }


                 // Update navigation links highlighting
                 updateNavLinks(pageId || (landingPage && landingPage.classList.contains('hidden') ? firstPageId : null));
            }

            function updateNavLinks(activePageId) {
                 // Update nav links within all page-nav elements
                 document.querySelectorAll('.page-nav a').forEach(link => {
                     if (link.dataset.page === activePageId) {
                         link.classList.add('active-nav-link');
                     } else {
                         link.classList.remove('active-nav-link');
                     }
                 });
            }

            navLinks.forEach(link => {
                link.addEventListener('click', (event) => {
                    const targetPageId = link.dataset.page;
                    if (targetPageId) {
                        // Check if the link is inside a page that is already active
                        // Only prevent default if it's a page switch link
                        const isPageSwitchLink = link.closest('.page') || link.classList.contains('enter-btn');
                        if (isPageSwitchLink) {
                             event.preventDefault(); // Prevent default anchor jump only for page switching links
                        }

                        showPage(targetPageId);
                        // Optional: Scroll to top of the new page
                        // Find the active page element to scroll within it if needed, or window
                         const activePageElement = document.querySelector('.page.active') || window;
                         activePageElement.scrollTo(0, 0);

                    }
                    // No special handling needed for explore button if it has data-page now
                });
            });

            // Initial state: Show landing page, hide others
            // Determine initial view based on URL hash or default
            const initialHash = window.location.hash.substring(1); // Get page id from URL like #page-skills
            const targetPageFromHash = document.getElementById(initialHash);

            if (targetPageFromHash && targetPageFromHash.classList.contains('page')) {
                 landingPage.classList.add('hidden');
                 showPage(initialHash);
            } else {
                // Default: Show landing page, hide others
                if (landingPage) {
                     landingPage.classList.remove('hidden');
                     pages.forEach(page => page.classList.remove('active'));
                } else {
                     // Fallback if landing page doesn't exist: show default page
                     showPage(firstPageId);
                }

            }

        });

        // Basic Visitor Counter (Example using localStorage)
        document.addEventListener('DOMContentLoaded', () => {
            const counterDisplays = document.querySelectorAll('.visitor-count-display');
            const storageKey = 'visitorCount_SujalPortfolio'; // Unique key

            if (counterDisplays.length > 0) {
                let count = localStorage.getItem(storageKey);
                count = count ? parseInt(count) : 0;

                // Simple increment on load - better methods exist for unique visits
                count++;

                localStorage.setItem(storageKey, count.toString());

                counterDisplays.forEach(display => {
                    display.textContent = count;
                });
            }
        });
    </script>

</body>
</html>
