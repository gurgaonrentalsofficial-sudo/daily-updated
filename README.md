<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Daily Update - हर खबर, सबसे पहले</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
}

body{
    font-family:Arial, sans-serif;
    background:#f4f4f4;
    color:#222;
}

header{
    background:#e60000;
    color:white;
    text-align:center;
    padding:20px 10px;
}

header h1{
    font-size:32px;
}

header p{
    margin-top:8px;
    font-size:18px;
}

nav{
    background:#111;
    display:flex;
    overflow-x:auto;
}

nav a{
    color:white;
    text-decoration:none;
    padding:15px 20px;
    white-space:nowrap;
}

nav a:hover{
    background:#e60000;
}

.breaking{
    background:white;
    padding:15px;
    font-size:18px;
    border-bottom:1px solid #ddd;
}

.breaking span{
    background:#e60000;
    color:white;
    padding:8px 12px;
    margin-right:10px;
}

.container{
    width:90%;
    max-width:1000px;
    margin:25px auto;
}

.hero{
    background:#ddd;
    height:220px;
    display:flex;
    align-items:center;
    justify-content:center;
    font-size:28px;
    margin-bottom:25px;
}

.section-title{
    font-size:28px;
    margin:20px 0;
    border-left:6px solid #e60000;
    padding-left:10px;
}

.news-grid{
    display:grid;
    grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
    gap:20px;
}

.news-card{
    background:white;
    border-radius:10px;
    overflow:hidden;
    box-shadow:0 2px 10px rgba(0,0,0,.1);
}

.news-card img{
    width:100%;
    height:170px;
    object-fit:cover;
}

.news-card-content{
    padding:15px;
}

.news-card h3{
    margin-bottom:10px;
}

.news-card p{
    color:#666;
    line-height:1.5;
}

.news-card button{
    margin-top:12px;
    background:#e60000;
    color:white;
    border:none;
    padding:8px 14px;
    border-radius:5px;
}

.admin-btn{
    position:fixed;
    bottom:20px;
    right:20px;
    background:#111;
    color:white;
    border:none;
    border-radius:50px;
    padding:15px 20px;
    font-size:16px;
}

.modal{
    display:none;
    position:fixed;
    inset:0;
    background:rgba(0,0,0,.6);
    padding:20px;
    overflow:auto;
}

.admin-box{
    background:white;
    max-width:500px;
    margin:30px auto;
    padding:20px;
    border-radius:10px;
}

.admin-box input,
.admin-box textarea,
.admin-box select{
    width:100%;
    padding:12px;
    margin:8px 0;
    border:1px solid #ccc;
    border-radius:5px;
}

.admin-box textarea{
    height:100px;
}

.admin-box button{
    padding:12px;
    margin-top:8px;
    border:none;
    border-radius:5px;
    background:#e60000;
    color:white;
    width:100%;
    font-size:16px;
}

.close{
    background:#555 !important;
}

.delete-btn{
    background:#111 !important;
    margin-top:10px;
}

footer{
    background:#111;
    color:white;
    text-align:center;
    padding:20px;
    margin-top:40px;
}

@media(max-width:600px){
    header h1{
        font-size:26px;
    }

    .hero{
        height:180px;
        font-size:22px;
    }
}
</style>
</head>

<body>

<header>
    <h1>📰 DAILY UPDATE</h1>
    <p>हर खबर, सबसे पहले</p>
</header>

<nav>
    <a href="#">होम</a>
    <a href="#">भारत</a>
    <a href="#">राज्य</a>
    <a href="#">राजनीति</a>
    <a href="#">बिजनेस</a>
    <a href="#">खेल</a>
</nav>

<div class="breaking">
    <span>BREAKING NEWS</span>
    आज की सबसे बड़ी और महत्वपूर्ण खबरें
</div>

<div class="container">

    <div class="hero">
        मुख्य खबर की तस्वीर
    </div>

    <h2 class="section-title">ताज़ा खबरें</h2>

    <div class="news-grid" id="newsGrid">
    </div>

</div>

<button class="admin-btn" onclick="openAdmin()">
    ⚙ Admin
</button>

<div class="modal" id="adminModal">

    <div class="admin-box">

        <h2>Admin Panel</h2>

        <input type="text" id="password" placeholder="Admin Password">

        <button onclick="login()">Login</button>

        <div id="adminContent" style="display:none;">

            <hr style="margin:20px 0;">

            <input type="text" id="newsTitle" placeholder="खबर का Title">

            <textarea id="newsDescription"
            placeholder="खबर का विवरण"></textarea>

            <input type="text" id="newsImage"
            placeholder="Image URL (optional)">

            <select id="newsCategory">
                <option>भारत</option>
                <option>राज्य</option>
                <option>राजनीति</option>
                <option>बिजनेस</option>
                <option>खेल</option>
            </select>

            <button onclick="addNews()">Publish News</button>

            <h3 style="margin-top:20px;">Published News</h3>

            <div id="adminNews"></div>

        </div>

        <button class="close" onclick="closeAdmin()">
            Close
        </button>

    </div>

</div>

<footer>
    © 2026 Daily Update | हर खबर, सबसे पहले
</footer>

<script>

const ADMIN_PASSWORD = "12345";

let news = JSON.parse(localStorage.getItem("dailyUpdateNews")) || [
    {
        title:"आज की सबसे बड़ी और महत्वपूर्ण खबर",
        description:"यहाँ आज की मुख्य खबर का छोटा विवरण दिखाई देगा।",
        image:"",
        category:"भारत"
    }
];

function renderNews(){

    const grid = document.getElementById("newsGrid");

    grid.innerHTML = "";

    news.forEach((item,index)=>{

        let imageHTML = item.image
        ? `<img src="${item.image}">`
        : `<div style="height:170px;background:#ddd;display:flex;align-items:center;justify-content:center;font-size:20px;">📰 खबर की तस्वीर</div>`;

        grid.innerHTML += `
        <div class="news-card">

            ${imageHTML}

            <div class="news-card-content">

                <small>${item.category}</small>

                <h3>${item.title}</h3>

                <p>${item.description}</p>

            </div>

        </div>
        `;
    });

}

function openAdmin(){
    document.getElementById("adminModal").style.display="block";
}

function closeAdmin(){
    document.getElementById("adminModal").style.display="none";
}

function login(){

    const password =
    document.getElementById("password").value;

    if(password === ADMIN_PASSWORD){

        document.getElementById("adminContent")
        .style.display="block";

        document.getElementById("password")
        .style.display="none";

        renderAdminNews();

    }else{

        alert("Wrong Password!");

    }

}

function addNews(){

    const title =
    document.getElementById("newsTitle").value;

    const description =
    document.getElementById("newsDescription").value;

    const image =
    document.getElementById("newsImage").value;

    const category =
    document.getElementById("newsCategory").value;

    if(title === "" || description === ""){

        alert("Title aur Description भरें");

        return;
    }

    news.unshift({
        title,
        description,
        image,
        category
    });

    localStorage.setItem(
        "dailyUpdateNews",
        JSON.stringify(news)
    );

    document.getElementById("newsTitle").value="";
    document.getElementById("newsDescription").value="";
    document.getElementById("newsImage").value="";

    renderNews();
    renderAdminNews();

    alert("News Published Successfully!");

}

function renderAdminNews(){

    const adminNews =
    document.getElementById("adminNews");

    adminNews.innerHTML="";

    news.forEach((item,index)=>{

        adminNews.innerHTML += `

        <div style="
        border:1px solid #ddd;
        padding:10px;
        margin-top:10px;
        border-radius:5px;">

        <b>${item.title}</b>

        <button class="delete-btn"
        onclick="deleteNews(${index})">

        Delete

        </button>

        </div>

        `;

    });

}

function deleteNews(index){

    if(confirm("क्या आप यह खबर Delete करना चाहते हैं?")){

        news.splice(index,1);

        localStorage.setItem(
            "dailyUpdateNews",
            JSON.stringify(news)
        );

        renderNews();
        renderAdminNews();

    }

}

renderNews();

</script>

</body>
</html>
