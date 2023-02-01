Type: Template
Title: Archive page

<!DOCTYPE html>
<html lang="en">
<head>
<title>{weblog-title} {separator} {post-title}</title>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="theme-color" content="#bd93f9" />
{feeds}
<style>
    @import url('https://static.omg.lol/type/font-lato-regular.css');
    @import url('https://static.omg.lol/type/font-lato-bold.css');
    @import url('https://static.omg.lol/type/font-lato-italic.css');
    @import url('https://static.omg.lol/type/font-md-io.css');
    @import url('https://static.omg.lol/type/fontawesome-free/css/all.css');
    @import url('https://cdn.cache.lol/profiles/icons/omg.lol-icons.css');
    @import url('https://utils.coryd.dev/fira-sans.css');
    @import url('https://utils.coryd.dev/blog.css');
    @import url('https://utils.coryd.dev/dracula.css');
    @font-face {
        font-family: 'omg.lol';
        src: url('data:font/woff2;charset=utf-8;base64,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') format('woff2');
        font-weight: 900;
        font-style: normal;
        font-display: swap;
    }
</style>
<link rel="apple-touch-icon" sizes="76x76" href="https://utils.coryd.dev/apple-touch-icon.png" />
<link rel="icon" type="image/png" sizes="32x32" href="https://utils.coryd.dev/favicon-32x32.png" />
<link rel="icon" type="image/png" sizes="16x16" href="https://utils.coryd.dev/favicon-16x16.png" />
<script defer data-domain=“blog.coryd.dev” src=“https://utils.coryd.dev/script.67532e4133a4t4551aza.js”></script>
<script src="https://utils.coryd.dev/highlight.js"></script>
<script>
    document.addEventListener('DOMContentLoaded', (event) => {
      document.querySelectorAll('pre code').forEach((el) => {
        hljs.highlightElement(el);
      });
    });
</script>
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
