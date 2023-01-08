Type: Template
Title: Archive

<!DOCTYPE html>
<html lang="en">
<head>
<title>{weblog-title} {separator} {post-title}</title>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="theme-color" content="#bd93f9" />
{feeds}
<style>
    @import url('https://static.omg.lol/type/font-honey.css');
    @import url('https://static.omg.lol/type/font-lato-regular.css');
    @import url('https://static.omg.lol/type/font-lato-bold.css');
    @import url('https://static.omg.lol/type/font-lato-italic.css');
    @import url('https://static.omg.lol/type/font-md-io.css');
    @import url('https://static.omg.lol/type/fontawesome-free/css/all.css');
    @import url('https://utils.coryd.dev/blog.css');
</style>
<link rel="apple-touch-icon" sizes="76x76" href="https://utils.coryd.dev/apple-touch-icon.png" />
<link rel="icon" type="image/png" sizes="32x32" href="https://utils.coryd.dev/favicon-32x32.png" />
<link rel="icon" type="image/png" sizes="16x16" href="https://utils.coryd.dev/favicon-16x16.png" />
<script defer data-domain="cory.weblog.lol" src="https://utils.coryd.dev/script.67532e4133a4t4551aza.js"></script>
</head>
<body>
<header>
    <h1 class="weblog-title"><a href="/">{weblog-title}</a></h1>
    {navigation}
</header>
<main>
    <article>
        <h1><i class="fa-solid fa-archive"></i> {post-title}</h1>
        {post-list}
    </article>
    <hr />
    <div class="weblog-info">
        <div class="container background-cyan">
            <h2><i class="fa-solid fa-clock"></i> Recent posts</h2>
            {recent-posts}
        </div>
        <div class="container background-purple">
            <h2><i class="fa-solid fa-house"></i> Home</h2>
            <p><a href="/">Head back home</a></p>
        </div>
        <div class="container background-orange">
            <h2><i class="fa-solid fa-note-sticky"></i> Status</h2>
            <script src="https://status.lol/cory.js?time&link&pretty&no-emoji"></script>
        </div>
        <div class="container background-pink">
            <h2><i class="fa-solid fa-fw fa-magnifying-glass"></i> Search</h2>
            <form class="weblog-search" action="?" method="get">
                <input placeholder="What are you looking for?" type="text" name="search">
                <div class="weblog-search--submit-wrap">
                    <button type="submit">Let's take a look!</button>
                </div>
            </form>
        </div>
    </div>
</main>

<footer>
    <p>Made with <a href="https://weblog.lol">weblog.lol</a>.</p>
</footer>

</body>
</html>