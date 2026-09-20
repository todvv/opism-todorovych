# Практична робота № 1

**Дисципліна:** Основи побудови інформаційних систем та мереж

**Тема:** Спостереження за процесом звернення до вебресурсу. Побудова власної моделі рівнів взаємодії

| | |
|---|---|
| **Прізвище, ім'я** | Тодорович Вікторія |
| **Група** | КБЗІ-2.02 |
| **Номер варіанта** | 26 |
| **Домен варіанта** | vntu.edu.ua |
| **Середовище виконання** | Windows 11 |
| **Версія curl** | curl 8.21.0 (Windows) libcurl/8.21.0 Schannel zlib/1.3.2 WinIDN WinLDAP |
| **Дата виконання** | 09.15.2026 |

---

## Частина A. Збір експериментальних даних

### A.1. Запит із діагностичним виводом

**Команда:**

```
curl -v https://vntu.edu.ua
```

**Вивід:**

```
* Host vntu.edu.ua:443 was resolved.
* IPv6: (none)
* IPv4: 194.146.143.10
*   Trying 194.146.143.10:443...
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* ALPN: server accepted http/1.1
* Established connection to vntu.edu.ua (194.146.143.10 port 443) from 192.168.0.102 port 50125
* using HTTP/1.x
> GET / HTTP/1.1
> Host: vntu.edu.ua
> User-Agent: curl/8.21.0
> Accept: */*
>
* Request completely sent off
* schannel: remote party requests renegotiation
* schannel: renegotiating SSL/TLS connection
* schannel: SSL/TLS connection renegotiated
* schannel: remote party requests renegotiation
* schannel: renegotiating SSL/TLS connection
* schannel: SSL/TLS connection renegotiated
< HTTP/1.1 200 OK
< Server: nginx
< Content-Type: text/html; charset=UTF-8
< Transfer-Encoding: chunked
< Connection: keep-alive
< Cache-Control: max-age=900, public
< Date: Tue, 15 Sep 2026 04:10:52 GMT
< X-UA-Compatible: IE=edge
< Content-language: uk
< X-Content-Type-Options: nosniff
< X-Frame-Options: SAMEORIGIN
< Permissions-Policy: interest-cohort=()
< Expires: Sun, 19 Nov 1978 05:00:00 GMT
< Last-Modified: Tue, 15 Sep 2026 04:10:51 GMT
< ETag: "1789445451"
< Vary: Cookie
< X-Powered-By: VNTU/JetIQ
<
<!DOCTYPE html>
<html xmlns:og="http://ogp.me/ns#" lang="uk">
<head>
  <meta charset="utf-8">
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Вінницький Національний Технічний Університет</title>
  <link rel='dns-prefetch' href='//cdnjs.cloudflare.com' />
  <link rel="alternate" type="application/rss+xml" title="" href="http://vntu.edu.ua/rss.xml" />
  <link rel="shortcut icon" href="/favicon.ico" type="image/x-icon">
  <meta property="og:url" content="https://vntu.edu.ua/index.html" />
  <meta property="og:title" content="Новини - Вінницький Національний Технічний Університет" />
  <meta property="og:description" content="ВНТУ - вищий навчальний заклад четвертого рівня акредитації, найбільший навчальний заклад в Подільському економічному регіоні" />
  <meta property="og:type" content="website" />
  <meta property="og:image" content="/style/vntu-preview.svg" />
  <meta name="description" content="ВНТУ - вищий навчальний заклад четвертого рівня акредитації, найбільший навчальний заклад в Подільському економічному регіоні" /><link rel="stylesheet" media="all" href="/sites/default/files/css/css_kOFx7RuDhn10qGc2ueK9FRUpABq6yYurnmChxjHHJzs.css" />
<link rel="stylesheet" media="all" href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.4.1/css/bootstrap.min.css" integrity="sha512-Dop/vW3iOtayerlYAqCgkVr2aTr2ErwwTYOvRFUpzl2VhCMJyjQF0Q9TjUXIo6JhuM/3i0vVEt2e/7QQmnHQqw==" crossorigin="anonymous" referrerpolicy="no-referrer" />
<link rel="stylesheet" media="all" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css" integrity="sha512-SfTiTlX6kk+qitfevl/7LibUOeJWlt9rbyDn92a1DqWOw9vWG2MFoays0sgObmWazO5BQPiFucnnEAjpAB+/Sw==" crossorigin="anonymous" referrerpolicy="no-referrer" />
<link rel="stylesheet" media="all" href="https://cdnjs.cloudflare.com/ajax/libs/slick-carousel/1.8.0/slick.min.css" integrity="sha512-yHknP1/AwR+yx26cB1y0cjvQUMvEa2PFzt1c9LlS4pRQ5NOTZFWbhBig+X9G9eYW/8m0/4OXNx8pxJ6z57x0dw==" crossorigin="anonymous" referrerpolicy="no-referrer" />
<link rel="stylesheet" media="all" href="/sites/default/files/css/css_8ZCKv1uMzFm1rKLKp9G2aSmUdKqDBfDeD6YDIhGxXEw.css" />
<link rel="stylesheet" media="all" href="/style/vntu.min.css?ver=2022013001" />


</head>
<body>

  <div class="dialog-off-canvas-main-canvas" data-off-canvas-main-canvas>
    <div class="header-line">
  <div class="container-fluid horzlimiter">
    <a id="vntu-logo" href="https://vntu.edu.ua/"></a>
    <nav class="navbar transparent navbar-inverse">
      <div class="container-fluid horzlimiter">
        <div class="collapse navbar-collapse">
          <div id="vntu-title" class="navbar-form navbar-left"><h1>Вінницький Національний Технічний Університет</h1></div>
          <div class="nav navbar-nav navbar-right buttons-box">
              <a href="/go/account" class="btn btn-primary" role="button">Особистий кабінет</a>
              <a href="/go/vstup_howto" class="btn btn-primary" role="button">Як вступити?</a>
          </div>        </div>
      </div>
    </nav>
  </div>
</div>
<nav id="vntu-navbar" class="navbar navbar-default">
  <div class="container-fluid horzlimiter leftlimiter">
    <div class="navbar-header">
      <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-navbar-collapse-1" aria-expanded="false">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
    </div>

        <div class="collapse navbar-collapse" id="bs-navbar-collapse-1">
        <div class="region region-navigation-collapsible">
    <nav role="navigation" aria-labelledby="block-vntu-main-menu-menu" id="block-vntu-main-menu">
            <h2 class="sr-only" id="block-vntu-main-menu-menu">Main navigation</h2>


      <ul class="nav navbar-nav navbar-hoverenable">
                      <li class="dropdown first">
                                                                            <a href="/"  class="dropdown-toggle" aria-haspopup="true" aria-expanded="false">Університет</a>
                        <ul class="dropdown-menu">
                      <li class="first">
                                                <a href="/" >Головна</a>
              </li>
                      <li>
                                                <a href="/uk/about-university/vntu-today.html" >Про університет</a>
              </li>
                      <li>
                                                <a href="/uk/about-university/administration.html" >Адмiнiстрацiя університету</a>
              </li>
                      <li>
                                                <a href="/uk/topic/reytingi-1505.html" >ВНТУ в рейтингах</a>
              </li>
                      <li>
                                                <a href="/uk/olimp.html" >Олімпіади та конкурси</a>
              </li>
                      <li>
                                                <a href="/lists/websites" >Інтернет-ресурси</a>
              </li>
                      <li>
                                                <a href="/uk/about-university/alumni.html" >Випускники</a>
              </li>
                      <li>
                                                <a href="http://lib.vntu.edu.ua/" >Бібліотека</a>
              </li>
                      <li class="last">
                                                <a href="/student-activities.html" >Студенту</a>
              </li>
        </ul>

              </li>
                      <li class="dropdown">
                                                                            <a href="https://vstup.vntu.edu.ua/"  class="dropdown-toggle" aria-haspopup="true" aria-expanded="false">Абітурієнту</a>
                        <ul class="dropdown-menu">
                      <li class="first">
                                                <a href="https://vstup.vntu.edu.ua/" >Портал приймальної комісії</a>
              </li>
                      <li>
                                                <a href="https://vstup.vntu.edu.ua/pravyla-pryiomu" >Правила прийому</a>
              </li>
                      <li>
                                                <a href="https://vstup.vntu.edu.ua/bakalavrat" > Вступ на бакалаврат</a>
              </li>
                      <li>
                                                <a href="https://vstup.vntu.edu.ua/mahistratura" > Вступ до магістратури</a>
              </li>
                      <li>
                                                <a href="https://ida.vntu.edu.ua/uk/vstupnykam-aspirantury/" >Вступ до аспірантури</a>
              </li>
                      <li>
                                                <a href="/information-for-enrollee/719.html" >Спеціальності бакалаврату</a>
              </li>
                      <li>
                                                <a href="/information-for-enrollee/1066.html" >Спеціальності магістратури</a>
              </li>
                      <li>
                                                <a href="/uk/prestudy/info.html" >Підготовчі курси НМТ</a>
              </li>
                      <li class="last">
                                                <a href="/uk/page/onlayn-intensyv-z-pidhotovky-do-tznk-2597.html" >Підготовчі курси ТЗНК</a>
              </li>
        </ul>

              </li>
                      <li class="dropdown">
                                                                            <a href="/uk/research.html"  class="dropdown-toggle" aria-haspopup="true" aria-expanded="false">Наука</a>
                        <ul class="dropdown-menu">
                      <li class="first">
                                                <a href="http://science.vntu.edu.ua/" >Науково-дослідна робота</a>
              </li>
                      <li>
                                                <a href="http://journals.vntu.edu.ua/" >Наукові журнали ВНТУ</a>
              </li>
                      <li>
                                                <a href="http://conferences.vntu.edu.ua/" >Науковi конференцiї ВНТУ</a>
              </li>
                      <li>
                                                <a href="https://ir.lib.vntu.edu.ua/" >Інституційний репозиторій</a>
              </li>
                      <li class="last">
                                                <a href="https://ida.vntu.edu.ua/" >Аспірантура та докторантура</a>
              </li>
        </ul>

              </li>
                      <li class="dropdown">
                                                                            <a href="/uk/education-work.html"  class="dropdown-toggle" aria-haspopup="true" aria-expanded="false">Освіта</a>
                        <ul class="dropdown-menu">
                      <li class="first">
                                                <a href="http://iq.vntu.edu.ua/" >Система підтримки навчального процесу JetIQ</a>
              </li>
                      <li>
                                                <a href="http://pams.vntu.edu.ua/" > Академічна мобільність</a>
              </li>
                      <li>
                                                <a href="https://jetiq.vntu.edu.ua/edu_progs/ep_list.php" >Освітні програми здобувачів вищої освіти</a>
              </li>
                      <li>
                                                <a href="http://ininv.vntu.edu.ua/" >Стажування та підвищення кваліфікації</a>
              </li>
                      <li>
                                                <a href="https://eqa.vntu.edu.ua/" >Забезпечення якості освіти</a>
              </li>
                      <li>
                                                <a href="http://cde.vntu.edu.ua/" >Дистанційна освіта</a>
              </li>
                      <li>
                                                <a href="https://posibnyky.vntu.edu.ua/" >Навчальні посібники</a>
              </li>
                      <li>
                                                <a href="https://iq.vntu.edu.ua/fm/fdb/682/akkr/akr.html" >Акредитація освітніх програм</a>
              </li>
                      <li>
                                                <a href="https://nv.vntu.edu.ua/" >Навчальний відділ</a>
              </li>
                      <li class="last">
                                                <a href="https://nv.vntu.edu.ua/?id=341&amp;mode=new_item&amp;f=682/nv_341/vartist.html" >Вартість навчання</a>
              </li>
        </ul>

              </li>
                      <li>
                                                <a href="https://int.vntu.edu.ua/" >Міжнародна діяльність</a>
              </li>
                      <li class="dropdown">
                                                                            <a href="/uk/institutes-and-departments.html"  class="dropdown-toggle" aria-haspopup="true" aria-expanded="false">Факультети та кафедри</a>
                        <ul class="dropdown-menu">
                      <li class="first">
                                                <a href="https://fiita.vntu.edu.ua/" >Факультет інтелектуальних інформаційних технологій та автоматизації </a>
              </li>
                      <li>
                                                <a href="https://bcei.vntu.edu.ua/" >Факультет будівництва, цивільної та екологічної інженерії</a>
              </li>
                      <li>
                                                <a href="http://feeem.vntu.edu.ua/" > Факультет електроенергетики та електромеханіки</a>
              </li>
                      <li>
                                                <a href="http://fitki.vntu.edu.ua/" >Факультет інформаційних технологій та комп&#039;ютерної інженерії</a>
              </li>
                      <li>
                                                <a href="http://fmt.vntu.edu.ua/" >Факультет машинобудування та транспорту</a>
              </li>
                      <li>
                                                <a href="http://inrtzp.vntu.edu.ua/" >Факультет інформаційних електронних систем</a>
              </li>
                      <li>
                                                <a href="http://fmib.vntu.edu.ua/" >Факультет менеджменту та інформаційної безпеки</a>
              </li>
                      <li class="last">
                                                <a href="https://jetiq.vntu.edu.ua/departs/index.php?id=347" >Кафедра військової підготовки</a>
              </li>
        </ul>

              </li>
                      <li class="last">
                                                <a href="/uk/contacts.html" >Контакти</a>
              </li>
        </ul>


  </nav>
      <ul id="language-selector" role="navigation" aria-labelledby="block-language-switcher-menu" class="language-switcher-language-url nav navbar-nav navbar-right nav-lang">
        <li class="searchbutton">
          <a href="#" class="dropdown-toggle dropdown-largeicon" data-toggle="dropdown" role="button" onclick="search_toggle()"><i class="fa fa-search"></i><span class="display-only-xs">&nbsp;&nbsp;Пошук</span></a>
        </li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle dropdown-largeicon" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false"><img src="/style/icons/lang.png" width="20" height="20" /><span class="display-only-xs">&nbsp;&nbsp;Мова</span></a>
                          <ul class="dropdown-menu"><li hreflang="uk" data-drupal-link-system-path="&lt;front&gt;" class="uk is-active"><a href="/" class="language-link is-active" hreflang="uk" data-drupal-link-system-path="&lt;front&gt;">Ukrainian</a></li><li hreflang="en" data-drupal-link-system-path="&lt;front&gt;" class="en"><a href="/en" class="language-link" hreflang="en" data-drupal-link-system-path="&lt;front&gt;">English</a></li></ul>
                    </li>
      </ul>

  </div>

    </div>
      </div><!-- /.container-fluid -->
</nav>

  <div class="slider-bg"><div class="container horzlimiter">
  <div id="vntu_slider" class="carousel slide" data-ride="carousel">
    <div id="vntu-social">
    <ul class="list-unstyled list-inline social text-center">
          <li class="list-inline-item"><a href="/go/facebook" target="_blank"><i class="fa fa-facebook"></i></a></li>      <li class="list-inline-item"><a href="/go/linkedin" target="_blank"><i class="fa fa-linkedin"></i></a></li>
      <li class="list-inline-item"><a href="/go/instagram" target="_blank"><i class="fa fa-instagram"></i></a></li>
      <li class="list-inline-item"><a href="/go/youtube" target="_blank"><i class="fa fa-youtube"></i></a></li>
      <li class="list-inline-item"><a href="/uk/contacts.html"><i class="fa fa-envelope"></i></a></li>
    </ul>
  </div>

  <div id="vntu-search-zone">
    <div class="poweredby">
      <a href="https://www.google.com/" target="_new"><img src="/style/icons/powered_by_google.png"></a>
    </div>
    <form class="search-form" data-drupal-selector="search-form" action="/search" method="post" id="search-form" accept-charset="UTF-8">
      <fieldset>
        <legend>Що ви бажаєте знайти в ВНТУ?</legend>
        <div class="inner-form">
          <div class="input-field">
            <div class="choices" data-type="text" aria-haspopup="true" aria-expanded="false" dir="ltr">
              <div class="choices-inner">
                <input id="edit-keys" class="choices-input" type="search" name="keys" value="" placeholder="Пошук по ключових словах" />
                <input data-drupal-selector="edit-search-form" type="hidden" name="form_id" value="search_form">
              </div>
            </div>
            <button class="btn-search" type="submit" name="op" value="Пошук"><i class="fa fa-search"></i></button>
          </div>
        </div>
        <div class="suggestion-wrap">
          <span><a href="/search?jsrc_type=advanced">Розширений пошук</a></span>
          <span><a href="#" onclick="search_toggle()">Закрити</a></span>
        </div>
        </fieldset>
    </form>
  </div>

  <ol class="carousel-indicators">
    <li data-target="#vntu_slider" data-slide-to="0" class="active"></li>
    <li data-target="#vntu_slider" data-slide-to="1"></li>
    <li data-target="#vntu_slider" data-slide-to="2"></li>
    <li data-target="#vntu_slider" data-slide-to="3"></li>
    <li data-target="#vntu_slider" data-slide-to="4"></li>
    <li data-target="#vntu_slider" data-slide-to="5"></li>
    <li data-target="#vntu_slider" data-slide-to="6"></li>
    <li data-target="#vntu_slider" data-slide-to="7"></li>
    <li data-target="#vntu_slider" data-slide-to="8"></li>
    <li data-target="#vntu_slider" data-slide-to="9"></li>
    <li data-target="#vntu_slider" data-slide-to="10"></li>
    <li data-target="#vntu_slider" data-slide-to="11"></li>
    <li data-target="#vntu_slider" data-slide-to="12"></li>
    <li data-target="#vntu_slider" data-slide-to="13"></li>
    <li data-target="#vntu_slider" data-slide-to="15"></li>
    <li data-target="#vntu_slider" data-slide-to="16"></li>

  </ol>

  <div class="carousel-inner">

    <div class="item active">
      <a href="/go/vstup_howto">
      <img src="/sites/contrib/slider/sl2.webp?ver=2022013003">
      <div class="carousel-caption d-none d-md-block">

      </div>
      </a>
    </div>


    <div class="item">
      <a href="/uk/student-activities.html">
      <img src="/sites/contrib/slider/sl3.webp?ver=2022013003">
      <div class="carousel-caption d-none d-md-block">
        <h5><b>Студентська спільнота</b></h5>
        <p>Студенти мають можливість долучатись до клубів та масштабних заходів</p>
      </div>
      </a>
    </div>

    <div class="item">
      <a href="/go/startup">
      <img src="/sites/contrib/slider/sl4.webp?ver=2022013004">
      <div class="carousel-caption d-none d-md-block">
              </div>
      </a>
    </div>

    <div class="item">
      <a href="#">
      <img src="/sites/contrib/slider/sl_img1.webp?ver=2026052904">
      <div class="carousel-caption d-none d-md-block">
              </div>
      </a>
    </div>

    <div class="item">
      <a href="#">
      <img src="/sites/contrib/slider/sl_img5.webp?ver=2026052904">
      <div class="carousel-caption d-none d-md-block">
              </div>
      </a>
    </div>

    <div class="item">
      <a href="#">
      <img src="/sites/contrib/slider/sl_img6.webp?ver=2026052904">
      <div class="carousel-caption d-none d-md-block">
              </div>
      </a>
    </div>

    <div class="item">
      <a href="#">
      <img src="/sites/contrib/slider/sl_img3.webp?ver=2026052904">
      <div class="carousel-caption d-none d-md-block">
              </div>
      </a>
    </div>

    <div class="item">
      <a href="#">
      <img src="/sites/contrib/slider/sl_img7.webp?ver=2026052904">
      <div class="carousel-caption d-none d-md-block">
              </div>
      </a>
    </div>

    <div class="item">
      <a href="#">
      <img src="/sites/contrib/slider/sl_img8.webp?ver=2026052904">
      <div class="carousel-caption d-none d-md-block">
              </div>
      </a>
    </div>

    <div class="item">
      <a href="#">
      <img src="/sites/contrib/slider/sl_img4.webp?ver=2026052904">
      <div class="carousel-caption d-none d-md-block">
              </div>
      </a>
    </div>

    <div class="item">
      <a href="#">
      <img src="/sites/contrib/slider/sl_img9.webp?ver=2026052904">
      <div class="carousel-caption d-none d-md-block">
              </div>
      </a>
    </div>

    <div class="item">
      <a href="#">
      <img src="/sites/contrib/slider/sl_img10.webp?ver=2026052904">
      <div class="carousel-caption d-none d-md-block">
              </div>
      </a>
    </div>

    <div class="item">
      <a href="#">
      <img src="/sites/contrib/slider/sl_img11.webp?ver=2026052904">
      <div class="carousel-caption d-none d-md-block">
              </div>
      </a>
    </div>

    <div class="item">
      <a href="#">
      <img src="/sites/contrib/slider/sl_img2.webp?ver=2026052904">
      <div class="carousel-caption d-none d-md-block">
              </div>
      </a>
    </div>

    <div class="item">
      <a href="#">
      <img src="/sites/contrib/slider/sl_img12.webp?ver=2026052904">
      <div class="carousel-caption d-none d-md-block">
              </div>
      </a>
    </div>

    <div class="item">
      <a href="#">
      <img src="/sites/contrib/slider/sl_img13.webp?ver=2026052904">
      <div class="carousel-caption d-none d-md-block">
              </div>
      </a>
    </div>


      </div>
  <a class="left carousel-control" href="#vntu_slider" data-slide="prev">
    <span class="glyphicon glyphicon-chevron-left"></span>
    <span class="sr-only">Previous</span>
  </a>
  <a class="right carousel-control" href="#vntu_slider" data-slide="next">
    <span class="glyphicon glyphicon-chevron-right"></span>
    <span class="sr-only">Next</span>
  </a>
</div>
  </div></div>

<div id="page-content">
<div class="container horzlimiter">
  <div id="page-vcontent">
    <div id="page-area" class="col-sm-8">
                                    <div id="page-rhelp">  <div id="vntu-spoilerbar">
    <span class="icon glyphicon glyphicon-question-sign" aria-hidden="true"></span>
<div id="block-vntu-helper" class="block block-block-content block-block-contentb2d47a91-53f9-48c9-807c-eac01a621637 clearfix">




            <div class="field field--name-body field--type-text-with-summary field--label-hidden field--item"><p><a href="https://vstup.vntu.edu.ua/">Портал приймальної комісії →</a><br /><br /><a href="https://vntu.edu.ua/uk/page/blahodiyna-orhanizatsiya-blahodiynyy-fond-vinnytska-politekhnika-1863.html">Благодійна організація "Благодійний фонд "Вінницька політехніка"" →</a></p>

<p><a href="https://docs.google.com/forms/d/e/1FAIpQLSdDbohJt7Xv7AFTXFuLxlFNfAZ2k6VHWLF6mQ0WXlsL4msg5A/viewform?usp=sharing">Замовлення довідки з ЄДЕБО для студентів та аспірантів ВНТУ →</a></p>

<p>Незабаром у ВНТУ:</p>
<!--<h4>Незабаром у ВНТУ:<br />
<br />
&nbsp;</h4>
--></div>

  </div>


  </div>
</div>

                  <div id="page-rhi" class="page-rhi-blocks">
              <div class="region region-highlighted-start">
    <div class="views-element-container block block-views block-views-blockpromo-sring-block-1 clearfix" id="block-promo-sring-block-main">

      <h4 class="block-title">Головне</h4>


      <div class="form-group"><div class="vntu-promo-news view view-promo-sring view-id-promo_sring view-display-id-block_1 js-view-dom-id-ff0d54cbbe3411ed02dc45330afaa163d5d40c472d30941d87c4db3c6a113eee">



      <div class="view-content">
          <div class="promo-banner"><div><div><div class="promo-image">
    <a href="/uk/news/z-dnem-mista-vinnytse-5319.html"><img src="/sites/default/files/promo/2026/09/photo_2026-09-12_11-52-00.jpg" alt="З Днем міста, Віннице!" /></a>
</div>
<div class="promo-subhead">
    <h4><a href="/uk/news/z-dnem-mista-vinnytse-5319.html">З Днем міста, Віннице!</a></h4>
</div></div></div></div>
    <div class="promo-banner"><div><div><div class="promo-image">
    <a href="/uk/news/vichna-pamyat-providniy-inzhenertsi-vntu-nini-petrivni-sonko-5321.html"><img src="/sites/default/files/promo/2026/09/photo_2026-09-12_18-09-27.jpg" alt="Вічна пам’ять провідній інженерці ВНТУ Ніні Петрівні Сонько" /></a>
</div>
<div class="promo-subhead">
    <h4><a href="/uk/news/vichna-pamyat-providniy-inzhenertsi-vntu-nini-petrivni-sonko-5321.html">Вічна пам’ять провідній інженерці ВНТУ Ніні Петрівні Сонько</a></h4>
</div></div></div></div>
    <div class="promo-banner"><div><div><div class="promo-image">
    <a href="/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-morozova-oleksandra-serhiyovycha-5317.html"><img src="/sites/default/files/promo/2026/09/Screenshot_1%20%282%29.jpg" alt="Захист дисертації на здобуття ступеня доктора філософії Морозова Олександра Сергійовича" /></a>
</div>
<div class="promo-subhead">
    <h4><a href="/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-morozova-oleksandra-serhiyovycha-5317.html">Захист дисертації на здобуття ступеня доктора філософії Морозова Олександра Сергійовича</a></h4>
</div></div></div></div>
    <div class="promo-banner"><div><div><div class="promo-image">
    <a href="/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-romantsya-vladyslava-olehovycha-5315.html"><img src="/sites/default/files/promo/2026/09/photo_2026-09-11_10-02-14%20%282%29.jpg" alt="Захист дисертації на здобуття ступеня доктора філософії Романця Владислава Олеговича" /></a>
</div>
<div class="promo-subhead">
    <h4><a href="/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-romantsya-vladyslava-olehovycha-5315.html">Захист дисертації на здобуття ступеня доктора філософії Романця Владислава Олеговича</a></h4>
</div></div></div></div>
    <div class="promo-banner"><div><div><div class="promo-image">
    <a href="/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-shvydkoho-dmytra-vasylovycha-5313.html"><img src="/sites/default/files/promo/2026/09/photo_2026-09-09_17-28-58.jpg" alt="Захист дисертації на здобуття ступеня доктора філософії Швидкого Дмитра Васильовича" /></a>
</div>
<div class="promo-subhead">
    <h4><a href="/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-shvydkoho-dmytra-vasylovycha-5313.html">Захист дисертації на здобуття ступеня доктора філософії Швидкого Дмитра Васильовича</a></h4>
</div></div></div></div>
    <div class="promo-banner"><div><div><div class="promo-image">
    <a href="/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-lyalyuka-andriya-oleksandrovycha-5311.html"><img src="/sites/default/files/promo/2026/09/Screenshot_3%20%282%29.jpg" alt="Захист дисертації на здобуття ступеня доктора філософії Лялюка Андрія Олександровича" /></a>
</div>
<div class="promo-subhead">
    <h4><a href="/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-lyalyuka-andriya-oleksandrovycha-5311.html">Захист дисертації на здобуття ступеня доктора філософії Лялюка Андрія Олександровича</a></h4>
</div></div></div></div>
    <div class="promo-banner"><div><div><div class="promo-image">
    <a href="/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-myloserdova-dmytra-andriyovycha-5309.html"><img src="/sites/default/files/promo/2026/09/photo_2026-09-08_17-55-14.jpg" alt="Захист дисертації на здобуття ступеня доктора філософії Милосердова Дмитра Андрійовича" /></a>
</div>
<div class="promo-subhead">
    <h4><a href="/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-myloserdova-dmytra-andriyovycha-5309.html">Захист дисертації на здобуття ступеня доктора філософії Милосердова Дмитра Андрійовича</a></h4>
</div></div></div></div>
    <div class="promo-banner"><div><div><div class="promo-image">
    <a href="/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-bondalyetova-kostyantyna-olehovycha-5307.html"><img src="/sites/default/files/promo/2026/09/photo_2026-09-08_17-52-55.jpg" alt="Захист дисертації на здобуття ступеня доктора філософії Бондалєтова Костянтина Олеговича" /></a>
</div>
<div class="promo-subhead">
    <h4><a href="/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-bondalyetova-kostyantyna-olehovycha-5307.html">Захист дисертації на здобуття ступеня доктора філософії Бондалєтова Костянтина Олеговича</a></h4>
</div></div></div></div>
    <div class="promo-banner"><div><div><div class="promo-image">
    <a href="/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-sandul-olhy-mykolayivny-5305.html"><img src="/sites/default/files/promo/2026/09/photo_1_2026-09-08_16-38-09.jpg" alt="Захист дисертації на здобуття ступеня доктора філософії Сандул Ольги Миколаївни" /></a>
</div>
<div class="promo-subhead">
    <h4><a href="/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-sandul-olhy-mykolayivny-5305.html">Захист дисертації на здобуття ступеня доктора філософії Сандул Ольги Миколаївни</a></h4>
</div></div></div></div>
    <div class="promo-banner"><div><div><div class="promo-image">
    <a href="/uk/news/barvy-ridnoho-podillya-vystavka-m-svatuly-5303.html"><img src="/sites/default/files/promo/2026/09/photo_3_2026-09-07_20-58-50.jpg" alt="«Барви рідного Поділля» виставка М. Сватули" /></a>
</div>
<div class="promo-subhead">
    <h4><a href="/uk/news/barvy-ridnoho-podillya-vystavka-m-svatuly-5303.html">«Барви рідного Поділля» виставка М. Сватули</a></h4>
</div></div></div></div>

    </div>

          </div>
</div>

  </div>


  </div>

            <h4 class="block-title">Новини університету</h4>
          </div>

                        <div id="page-rhead" role="heading">  <div class="region region-header">
    <div data-drupal-messages-fallback class="hidden"></div>

  </div>
</div>

              <div id="page-rcontent">  <div class="region region-content">
      <div class="views-element-container form-group"><div class="vntu-news view view-index view-id-index view-display-id-vntu_news js-view-dom-id-46779e5416b33edea3eb3552206bd5bd6a8835f06561138edf4350a937a3df04">



      <div class="view-content">
      <div data-drupal-views-infinite-scroll-content-wrapper class="views-infinite-scroll-content-wrapper clearfix form-group"><div class="item-list">
      <h3>12 вересня</h3>

  <ul class="vntu-news-list">

          <li class="vntu-news-row"><div class="views-field views-field-timestamp-1"><span class="field-content">15:39</span></div><div class="views-field views-field-title"><span class="field-content"><a href="https://vntu.edu.ua/uk/news/z-dnem-mista-vinnytse-5319.html">З Днем міста, Віннице!</a></span></div></li>
          <li class="vntu-news-row"><div class="views-field views-field-timestamp-1"><span class="field-content">10:55</span></div><div class="views-field views-field-title"><span class="field-content"><a href="https://vntu.edu.ua/uk/news/vichna-pamyat-providniy-inzhenertsi-vntu-nini-petrivni-sonko-5321.html">Вічна пам’ять провідній інженерці ВНТУ Ніні Петрівні Сонько</a></span></div></li>

  </ul>

</div>
<div class="item-list">
      <h3>11 вересня</h3>

  <ul class="vntu-news-list">

          <li class="vntu-news-row"><div class="views-field views-field-timestamp-1"><span class="field-content">15:25</span></div><div class="views-field views-field-title"><span class="field-content"><a href="https://vntu.edu.ua/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-morozova-oleksandra-serhiyovycha-5317.html">Захист дисертації на здобуття ступеня доктора філософії Морозова Олександра Сергійовича</a></span></div></li>
          <li class="vntu-news-row"><div class="views-field views-field-timestamp-1"><span class="field-content">15:06</span></div><div class="views-field views-field-title"><span class="field-content"><a href="https://vntu.edu.ua/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-romantsya-vladyslava-olehovycha-5315.html">Захист дисертації на здобуття ступеня доктора філософії Романця Владислава Олеговича</a></span></div></li>

  </ul>

</div>
<div class="item-list">
      <h3>10 вересня</h3>

  <ul class="vntu-news-list">

          <li class="vntu-news-row"><div class="views-field views-field-timestamp-1"><span class="field-content">12:44</span></div><div class="views-field views-field-title"><span class="field-content"><a href="https://vntu.edu.ua/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-shvydkoho-dmytra-vasylovycha-5313.html">Захист дисертації на здобуття ступеня доктора філософії Швидкого Дмитра Васильовича</a></span></div></li>
          <li class="vntu-news-row"><div class="views-field views-field-timestamp-1"><span class="field-content">12:38</span></div><div class="views-field views-field-title"><span class="field-content"><a href="https://vntu.edu.ua/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-lyalyuka-andriya-oleksandrovycha-5311.html">Захист дисертації на здобуття ступеня доктора філософії Лялюка Андрія Олександровича</a></span></div></li>

  </ul>

</div>
<div class="item-list">
      <h3>9 вересня</h3>

  <ul class="vntu-news-list">

          <li class="vntu-news-row"><div class="views-field views-field-timestamp-1"><span class="field-content">09:33</span></div><div class="views-field views-field-title"><span class="field-content"><a href="https://vntu.edu.ua/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-myloserdova-dmytra-andriyovycha-5309.html">Захист дисертації на здобуття ступеня доктора філософії Милосердова Дмитра Андрійовича</a></span></div></li>
          <li class="vntu-news-row"><div class="views-field views-field-timestamp-1"><span class="field-content">09:08</span></div><div class="views-field views-field-title"><span class="field-content"><a href="https://vntu.edu.ua/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-bondalyetova-kostyantyna-olehovycha-5307.html">Захист дисертації на здобуття ступеня доктора філософії Бондалєтова Костянтина Олеговича</a></span></div></li>

  </ul>

</div>
<div class="item-list">
      <h3>8 вересня</h3>

  <ul class="vntu-news-list">

          <li class="vntu-news-row"><div class="views-field views-field-timestamp-1"><span class="field-content">16:39</span></div><div class="views-field views-field-title"><span class="field-content"><a href="https://vntu.edu.ua/uk/news/zakhyst-dysertatsiyi-na-zdobuttya-stupenya-doktora-filosofiyi-sandul-olhy-mykolayivny-5305.html">Захист дисертації на здобуття ступеня доктора філософії Сандул Ольги Миколаївни</a></span></div></li>

  </ul>

</div>
<div class="item-list">
      <h3>7 вересня</h3>

  <ul class="vntu-news-list">

          <li class="vntu-news-row"><div class="views-field views-field-timestamp-1"><span class="field-content">20:58</span></div><div class="views-field views-field-title"><span class="field-content"><a href="https://vntu.edu.ua/uk/news/barvy-ridnoho-podillya-vystavka-m-svatuly-5303.html">«Барви рідного Поділля» виставка М. Сватули</a></span></div></li>

  </ul>

</div>
</div>

    </div>


<ul class="js-pager__items pager" data-drupal-views-infinite-scroll-pager>
  <li class="pager__item">
    <a class="button" href="?page=1" title="Load more items" rel="next">+ завантажити ще</a>
  </li>
</ul>

          </div>
</div>


  </div>
</div>


                        <div id="page-rhi-bottom" class="page-rhi-blocks">  <div class="region region-highlighted">
    <div id="block-jetiq" class="block block-jetiq block-jetiq-block clearfix">

      <h4 class="block-title">Новини факультетів та кафедр</h4>


      <div class="iq-news"><div class="iq-peview"><a href="https://iq.vntu.edu.ua/departs/index.php?id=246&amp;id_news=3513&amp;mode=full_news"><img class="iq-thumbnail" src="/jetiq/getpic.do?id_news=3513" /></a></div><div class="iq-fields"><span class="iq-datetime"><i class="fa fa-clock-o"></i> 2026.09.14</span>&nbsp;<span class="iq-person"><i class="fa fa-user"></i> Каплун Валентина Аполінаріївна, </span><span class="iq-department">Кафедра Захисту інформації</span><a class="iq-link" href="https://iq.vntu.edu.ua/departs/index.php?id=246&amp;id_news=3513&amp;mode=full_news"><div class="iq-text">Колектив кафедри ЗІ вітає своїх аспірантів з успішним захистом дисертації та здобуттям ступеня доктора філософії!  Колектив кафедри захисту інформації щиро вітає аспірантів кафедри Селезньова Віталія Ігоровича та Ціхоцького Микиту Сергійовича з успішним захистом дисертацій на здобуття ступеня доктора філософії з галузі знань 12 «Інформаційні технології» за спеціальністю 125 «Кібербезпека».</div></a></div></div>
<div class="iq-news"><div class="iq-peview"><a href="https://iq.vntu.edu.ua/departs/index.php?id=391&amp;id_news=3512&amp;mode=full_news"><img class="iq-thumbnail" src="/jetiq/getpic.do?id_news=3512" /></a></div><div class="iq-fields"><span class="iq-datetime"><i class="fa fa-clock-o"></i> 2026.09.13</span>&nbsp;<span class="iq-person"><i class="fa fa-user"></i> Бондар Альона Василівна, </span><span class="iq-department">Кафедра Міського будівництва та архітектури</span><a class="iq-link" href="https://iq.vntu.edu.ua/departs/index.php?id=391&amp;id_news=3512&amp;mode=full_news"><div class="iq-text">«Не вкрадене. Херсон»: майбутні архітектори та містобудівники ВНТУ долучилися до відкриття виставки Студенти архітектори та містобудівники разом із викладачами кафедри МБА відвідали відкриття виставки «Не вкрадене. Херсон» у Художньому музеї Вінниці. Це подія про пам’ять, культурну спадщину та силу зберігати своє навіть у часи руйнувань. Виставка дає можливість по-новому осмислити цінність міста, його архітектури, історії та культурної ідентичності.</div></a></div></div>
<div class="iq-news"><div class="iq-peview"><a href="https://iq.vntu.edu.ua/departs/index.php?id=234&amp;id_news=3511&amp;mode=full_news"><img class="iq-thumbnail" src="/jetiq/getpic.do?id_news=3511" /></a></div><div class="iq-fields"><span class="iq-datetime"><i class="fa fa-clock-o"></i> 2026.09.11</span>&nbsp;<span class="iq-person"><i class="fa fa-user"></i> Арсенюк Ігор Ростиславович, </span><span class="iq-department">Кафедра Комп`ютерних наук</span><a class="iq-link" href="https://iq.vntu.edu.ua/departs/index.php?id=234&amp;id_news=3511&amp;mode=full_news"><div class="iq-text">Колектив ФІІТА щиро вітає аспіранта кафедри комп’ютерних наук Морозова О. С. (а також його наукового керівника – д. т. н., проф. Ярового А. А.) із успішним захистом дисертації «Інтелектуальна інформаційна технологія автоматизованого тестування web-орієнтованого програмного забезпечення» на здобуття ступеня доктора філософії з галузі знань 12 «Інформаційні технології» за спеціальністю 122 «Комп’ютерні науки», що відбувся 10 вересня 2026 р.</div></a></div></div>
<div class="iq-news"><div class="iq-peview"><a href="https://iq.vntu.edu.ua/departs/index.php?id=317&amp;id_news=3509&amp;mode=full_news"><img class="iq-thumbnail" src="/jetiq/getpic.do?id_news=3509" /></a></div><div class="iq-fields"><span class="iq-datetime"><i class="fa fa-clock-o"></i> 2026.09.09</span>&nbsp;<span class="iq-person"><i class="fa fa-user"></i> Коваленко Олена Олексіївна, </span><span class="iq-department">Центр діджиталізації освітнього процесу</span><a class="iq-link" href="https://iq.vntu.edu.ua/departs/index.php?id=317&amp;id_news=3509&amp;mode=full_news"><div class="iq-text">JetTeX - використовуємо міжнародні стандарти для роботи з науковими публікаціями, випусковими роботами, звітами... Тепер викладачі та студенти мають доступ до міжнародного редактора з готовими шаблонами в персональному кабінеті. На відміну від Word, який працює за принципом WYSIWYG («що бачиш, те й отримаєш»), LaTeX працює за принципом WYSIWYM («що маєш на увазі, те й отримаєш»).</div></a></div></div>
<div class="iq-block-sidelink"><a href="/jetiq/news">Всі новини &rarr;</a></div>
  </div>


  </div>
</div>
                  </div>
    <div id="sidebar-right" class="col-sm-4">
                    <div class="region region-sidebar-first">
    <div class="block" role="navigation" aria-labelledby="block-navigaciya-menu" id="block-navigaciya">

  <h4 class="menu-theader" id="block-navigaciya-menu">Навігація</h4>



      <ul class="menu menu--navigation nav">
                      <li class="first">
                                                <a href="/" >Головна</a>
              </li>
                      <li>
                                                                                                <a href="#id1442"  class="collapsed" data-toggle="collapse">Університет</a>
                        <ul class="right-links collapse" id="id1442">
                      <li class="first">
                                                <a href="/uk/about-university/vntu-today.html" >Про нас</a>
              </li>
                      <li>
                                                <a href="/uk/about-university/history.html" >Історія університету</a>
              </li>
                      <li>
                                                <a href="/images/docs/vntustatut.pdf" >Статут ВНТУ</a>
              </li>
                      <li>
                                                <a href="/uk/about-university/administration.html" >Адмiнiстрацiя університету</a>
              </li>
                      <li>
                                                <a href="/uk/page/profesory-universytetu-2-3370.html" >Професори Університету</a>
              </li>
                      <li>
                                                                                                <a href="#id9139"  class="collapsed" data-toggle="collapse">Загальний відділ «Офіс ректора»</a>
                        <ul class="right-links collapse" id="id9139">
                      <li class="first">
                                                <a href="/about-office" >Про «Офіс ректора»</a>
              </li>
                      <li class="last">
                                                <a href="/archive-office" >Замовлення архівної довідки/витягу</a>
              </li>
        </ul>

              </li>
                      <li>
                                                                                                <a href="#id5717"  class="collapsed" data-toggle="collapse">Вчена рада</a>
                        <ul class="right-links collapse" id="id5717">
                      <li class="first">
                                                <a href="https://vntu.edu.ua/uploads/2025/ListOfMembers_VR.pdf" >Склад Вченої ради ВНТУ</a>
              </li>
                      <li>
                                                <a href="/uk/page/plan-roboty-vchenoyi-rady-2470.html" >План роботи Вченої ради</a>
              </li>
                      <li>
                                                <a href="/uk/page/rishennya-vchenoyi-rady-1895.html" >Рішення Вченої ради</a>
              </li>
                      <li>
                                                <a href="/page/vchenaradastate" >Положення Вченої ради</a>
              </li>
                      <li>
                                                <a href="/uk/page/perelik-dokumentiv-na-obrannya-npp-1975.html" >Перелік документів на обрання НПП</a>
              </li>
                      <li class="last">
                                                                                                <a href="#id7310"  class="collapsed" data-toggle="collapse">Присвоєння вчених звань</a>
                        <ul class="right-links collapse" id="id7310">
                      <li class="first">
                                                <a href="/uk/page/zrazky-ta-formy-dokumentiv-na-prysvoyennya-vchenoho-zvannya-dotsenta-1971.html" >Присвоєння вченого звання доцента</a>
              </li>
                      <li class="last">
                                                <a href="/uk/page/zrazky-ta-formy-dokumentiv-shchodo-prysvoyennya-vchenoho-zvannya-profesora-1972.html" >Присвоєння вченого звання професора</a>
              </li>
        </ul>

              </li>
        </ul>

              </li>
                      <li>
                                                <a href="/uk/page/pochesni-profesory-vntu-2-3327.html" >Почесні професори ВНТУ</a>
              </li>
                      <li>
                                                <a href="/uk/topic/reytingi-1505.html" >ВНТУ в рейтингах</a>
              </li>
                      <li>
                                                <a href="/konkurs-posad.html" >Вакансії</a>
              </li>
                      <li>
                                                <a href="/information-for-enrollee/license.html" >Ліцензія</a>
              </li>
                      <li>
                                                <a href="/uk/topic/brendbuk-1423.html" >Брендбук</a>
              </li>
                      <li class="last">
                                                <a href="/uk/topic/umovi-dostupnosti-vntu-dlya-navchannya-osib-z-osoblivimi-osvitnimi-potrebami-1385.html" >Умови доступності для навчання осіб з особливими освітніми потребами</a>
              </li>
        </ul>

              </li>
                      <li>
                                                <a href="https://vstup.vntu.edu.ua/" >Довідка абітурієнта</a>
              </li>
                      <li>
                                                                                                <a href="#id7485"  class="collapsed" data-toggle="collapse">Олімпіади та конкурси</a>
                        <ul class="right-links collapse" id="id7485">
                      <li class="first">
                                                <a href="https://olimp.vntu.edu.ua/uk/icpc/" >Олімпіада ICPC (SEERC)</a>
              </li>
                      <li>
                                                <a href="https://olimp.vntu.edu.ua/uk/aucpc-obl/" >Всеукраїнська студентська олімпіада з програмування 1 (обласний) етап</a>
              </li>
                      <li>
                                                <a href="https://olimp.vntu.edu.ua/uk/aucpc-reg/" >Всеукраїнська студентська олімпіада з програмування 2 (регіональний) етап</a>
              </li>
                      <li>
                                                <a href="http://webdesign.vntu.edu.ua/" >Конкурс з веб-дизайну ВНТУ</a>
              </li>
                      <li>
                                                <a href="/uk/contest/linux.html" >Конкурс &quot;Адміністрування Linux&quot;</a>
              </li>
                      <li>
                                                <a href="http://vsort.vntu.edu.ua/" >Всеукраїнська студентська олімпіада з радіотехніки</a>
              </li>
                      <li class="last">
                                                <a href="/uk/olimp.html" >Всі олімпіади та конкурси</a>
              </li>
        </ul>

              </li>
                      <li>
                                                <a href="/uk/institutes-and-departments.html" >Факультети та кафедри</a>
              </li>
                      <li>
                                                                                                <a href="#id6480"  class="collapsed" data-toggle="collapse">Науково-дослідна робота</a>
                        <ul class="right-links collapse" id="id6480">
                      <li class="first">
                                                <a href="http://journals.vntu.edu.ua/" >Наукові журнали</a>
              </li>
                      <li>
                                                <a href="http://conferences.vntu.edu.ua/" >Науковi конференцiї</a>
              </li>
                      <li class="last">
                                                <a href="http://science.vntu.edu.ua/" >Головний науковий сайт</a>
              </li>
        </ul>

              </li>
                      <li>
                                                <a href="https://nmr.vntu.edu.ua/" >Навчально-методична робота</a>
              </li>
                      <li>
                                                                                                <a href="#id9106"  class="collapsed" data-toggle="collapse">Аспірантура та докторантура</a>
                        <ul class="right-links collapse" id="id9106">
                      <li class="first">
                                                <a href="https://ida.vntu.edu.ua/" >Інститут докторантури та аспірантури</a>
              </li>
                      <li class="last">
                                                <a href="https://ida.vntu.edu.ua/uk/spetsrady/" >Робота спецрад</a>
              </li>
        </ul>

              </li>
                      <li>
                                                <a href="https://int.vntu.edu.ua/" >Міжнародна діяльність</a>
              </li>
                      <li>
                                                <a href="/uk/publishing-activities.html" >Видавнича діяльність</a>
              </li>
                      <li>
                                                                                                <a href="#id7844"  class="collapsed" data-toggle="collapse">Публічна інформація</a>
                        <ul class="right-links collapse" id="id7844">
                      <li class="first">
                                                <a href="/uk/public-info.html" >Каталог публічної інформації</a>
              </li>
                      <li>
                                                <a href="/public-info/zag.html" >Загальна публічна інформація</a>
              </li>
                      <li>
                                                <a href="/rector_election" >Вибори ректора – 2026</a>
              </li>
                      <li>
                                                <a href="/public-info/2014.html" >2014 рік</a>
              </li>
                      <li>
                                                <a href="/public-info/2015.html" >2015 рік</a>
              </li>
                      <li>
                                                <a href="/public-info/public2016.html" >2016 рік</a>
              </li>
                      <li>
                                                <a href="/public-info/public2017.html" >2017 рік</a>
              </li>
                      <li>
                                                <a href="/public-info/public2018.html" >2018 рік</a>
              </li>
                      <li>
                                                <a href="/public-info/public2019.html" >2019 рік</a>
              </li>
                      <li>
                                                <a href="/uk/public-info/publichna-informaciya-2020-926.html" >2020 рік</a>
              </li>
                      <li>
                                                <a href="/uk/topic/publichna-informaciya-2021-1057.html" >2021 рік</a>
              </li>
                      <li>
                                                <a href="/uk/topic/publichna-informaciya-2022-1246.html" >2022 рік</a>
              </li>
                      <li>
                                                <a href="/uk/page/publichna-informatsiya-2023-1745.html" >2023 рік</a>
              </li>
                      <li>
                                                <a href="/uk/page/publichna-informatsiya-2024-2437.html" >2024 рік</a>
              </li>
                      <li>
                                                <a href="/uk/page/publichna-informatsiya-2025-3380.html" >2025 рік</a>
              </li>
                      <li>
                                                <a href="/publicinfo2026" >2026 рік</a>
              </li>
                      <li>
                                                <a href="/uk/topic/zapobigannya-korupcii-996.html" >Запобігання корупції</a>
              </li>
                      <li class="last">
                                                <a href="/uk/page/dohovory-orendy-1581.html" >Оренда</a>
              </li>
        </ul>

              </li>
                      <li>
                                                <a href="https://ir.lib.vntu.edu.ua/?locale-attribute=uk" >Інституційний репозиторій</a>
              </li>
                      <li>
                                                <a href="/uk/page/heroyi-ne-vmyrayut-1635.html" >Герої не вмирають</a>
              </li>
                      <li>
                                                <a href="https://imagecenter.vntu.edu.ua/" >Імідж-центр</a>
              </li>
                      <li>
                                                <a href="http://lib.vntu.edu.ua/" >Бібліотека</a>
              </li>
                      <li>
                                                <a href="http://photo.vntu.edu.ua/" >Фотогалерея</a>
              </li>
                      <li>
                                                                                                <a href="#id7743"  class="collapsed" data-toggle="collapse">Випускникам</a>
                        <ul class="right-links collapse" id="id7743">
                      <li class="first">
                                                <a href="/uk/about-university/alumni.html" >Спільнота випускників</a>
              </li>
                      <li>
                                                <a href="/uk/dovidka/employers.html" >Практика на підприємствах</a>
              </li>
                      <li>
                                                <a href="/uk/dovidka/reviews.html" >Відгуки про навчальний заклад</a>
              </li>
                      <li class="last">
                                                <a href="/uk/page/vyhotovlennya-dublikativ-dokumentiv-pro-osvitu-1985.html" >Виготовлення дублікатів документів про освіту</a>
              </li>
        </ul>

              </li>
                      <li>
                                                <a href="/news/archive" >Архів новин</a>
              </li>
                      <li>
                                                <a href="/jetiq/sitemap" >Карта сайту</a>
              </li>
                      <li class="last">
                                                <a href="/uk/contacts.html" >Контакти</a>
              </li>
        </ul>


  </div>
<div class="block" role="navigation" aria-labelledby="block-organizaton-menu" id="block-organizaton">

  <h4 class="menu-theader" id="block-organizaton-menu">Організація</h4>



      <ul class="menu menu--organizaton nav">
                      <li class="first">
                                                <a href="/lists/websites" >Веб-сайти університету</a>
              </li>
                      <li>
                                                                                                <a href="#id4178"  class="collapsed" data-toggle="collapse">Проєкти університету</a>
                        <ul class="right-links collapse" id="id4178">
                      <li class="first">
                                                <a href="http://3dvin.vntu.edu.ua/" >3D Вінниця</a>
              </li>
                      <li>
                                                <a href="/energo.html" >Вища освіта України</a>
              </li>
                      <li>
                                                <a href="/uk/topic/atestaciyna-komisiya-vntu-876.html" >Енергоефективність та енергозбереження</a>
              </li>
                      <li class="last">
                                                                                                <a href="#id2016"  class="collapsed" data-toggle="collapse">Програми підвищення кваліфікації</a>
                        <ul class="right-links collapse" id="id2016">
                      <li class="first last">
                                                <a href="/uk/topic/programi-pidvischennya-kvalifikacii-derzhavnikh-sluzhbovciv-kategoriy-a-b-i-v-z-elektroenergetichnogo-napryamku-ta-teplopostachannya-1354.html" >Програми підвищення кваліфікації державних службовців категорій «А», «Б» і «В» з електроенергетичного напрямку та теплопостачання</a>
              </li>
        </ul>

              </li>
        </ul>

              </li>
                      <li>
                                                                                                <a href="#id5669"  class="collapsed" data-toggle="collapse">Підрозділи університету</a>
                        <ul class="right-links collapse" id="id5669">
                      <li class="first">
                                                <a href="http://socio-lab.vntu.edu.ua/" >Лабораторія соціологічних досліджень ВНТУ</a>
              </li>
                      <li>
                                                <a href="http://citzi.vntu.edu.ua/" >Центр інформаційних технологій і захисту інформації</a>
              </li>
                      <li class="last">
                                                <a href="/go/departments" >Всі підрозділи</a>
              </li>
        </ul>

              </li>
                      <li>
                                                <a href="https://confucius.vntu.edu.ua/" >Інститут Конфуція</a>
              </li>
                      <li>
                                                <a href="/uk/page/blahodiyna-orhanizatsiya-blahodiynyy-fond-vinnytska-politekhnika-1863.html" >Благодійний фонд &quot;Вінницька політехніка&quot;</a>
              </li>
                      <li>
                                                <a href="https://soc.vntu.edu.ua/" >Центр соціально-організаційної роботи</a>
              </li>
                      <li>
                                                <a href="http://ininv.vntu.edu.ua/" >Центр розвитку кар’єри та неперервної освіти</a>
              </li>
                      <li>
                                                <a href="/uk/page/okhorona-pratsi-ta-tsyvilnyy-zakhyst-2503.html" >Охорона праці та цивільний захист</a>
              </li>
                      <li>
                                                                                                <a href="#id2012"  class="collapsed" data-toggle="collapse">Студентські організації</a>
                        <ul class="right-links collapse" id="id2012">
                      <li class="first">
                                                <a href="http://sts.vntu.edu.ua/" >Студентське самоврядування</a>
              </li>
                      <li>
                                                <a href="http://ppos.vntu.edu.ua/" >Профспілка студентів ВНТУ</a>
              </li>
                      <li class="last">
                                                <a href="https://scsp.vntu.edu.ua/" >Наукове товариство студентів та аспірантів</a>
              </li>
        </ul>

              </li>
                      <li>
                                                                                                <a href="#id9256"  class="collapsed" data-toggle="collapse">Громадські організації</a>
                        <ul class="right-links collapse" id="id9256">
                      <li class="first">
                                                <a href="http://bf.vntu.edu.ua/" >Благодійний фонд ВНТУ</a>
              </li>
                      <li>
                                                <a href="/2016-11-18-11-17-05.html" >Конференція трудового колективу</a>
              </li>
                      <li>
                                                <a href="http://ardvin.org.ua/" >Агенція регіонального розвитку Вінницької області</a>
              </li>
                      <li class="last">
                                                <a href="/lists/organizations" >Інші організації</a>
              </li>
        </ul>

              </li>
                      <li>
                                                <a href="https://soc.vntu.edu.ua/?id=332&amp;mode=new_item&amp;f=sites/332/psychology.html" >Психологічна служба</a>
              </li>
                      <li>
                                                <a href="/uk/topic/skrinya-doviri-959.html" >Скриня довіри</a>
              </li>
                      <li class="last">
                                                <a href="https://edms.vntu.edu.ua/c_e.php" >Календар подій</a>
              </li>
        </ul>


  </div>
<div id="block-vntu-privat-banner" class="block block-block-content block-block-contentf773e06b-73ea-4b04-8eba-08a4e5fe3f1b clearfix">




            <div class="field field--name-body field--type-text-with-summary field--label-hidden field--item"><p><a class="document-link" href="/go/privat24" style="background-image: url(/style/icons/ap24.png);" target="_new"><span style="color: #000 !important;">Оплачуйте навчання та проживання в гуртожитках за допомогою</span> <span style="color: #348401 !important;">Приват24</span></a></p>
</div>

  </div>

<div id="block-vntu-payvn" class="block block-block-content block-block-content08be5cf3-024e-4a28-9060-f99d248ea6cf clearfix">




            <div class="field field--name-body field--type-text-with-summary field--label-hidden field--item"><p><a class="document-link" href="https://pay.vn.ua/additional_services/payment_form?group_name=%D0%92%D1%96%D0%BD%D0%BD%D0%B8%D1%86%D1%8C%D0%BA%D0%B8%D0%B9+%D0%9D%D0%A2%D0%A3&amp;other_pay_id=909" style="background-image: url(/style/icons/payvn.png);" target="_new"><span style="color: #000 !important;">Оплата за гуртожиток, оплата за навчання та за оренду за допомогою</span> <span style="color: #B12F2F !important;">Pay.vn.ua</span></a></p>
</div>

  </div>


  </div>

              </div>
  </div>
</div>
</div>

<div id="banners-jtron" class="jumbotron">
  <div class="container horzlimiter text-center">
      <div class="region region-footer">
    <div id="block-vntu-banners-2" class="block block-block-content block-block-content7a83895a-aa73-48b1-a48a-a91388b8e829 clearfix">




            <div class="field field--name-body field--type-text-with-summary field--label-hidden field--item"><div class="banner-slider">
    <!--<div><a href="https://mastis.pro/" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/mastis.png" /></a></div>-->
    <div><a href="http://startup.vntu.edu.ua/" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/sik.png" /></a></div>
    <div><a href="https://confucius.vntu.edu.ua/" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/confucius.png" /></a></div>
    <div><a href="/go/energoau" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/energoau-1.png" /></a></div>
   <!--<div><a href="https://t.me/vntucareer" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/vntu-vacantion.png" /></a></div>-->
    <div><a href="https://www.facebook.com/a.integrityVNTU/" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/belka.png" /></a></div>
    <div><a href="http://www.britishcouncil.org.ua/" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/bc.png" /></a></div>
    <div><a href="https://www.datagroup.ua/" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/dg-new.png" /></a></div>
    <div><a href="https://www.awsacademy.com/" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/aws-acad.png" /></a></div>
    <div><a href="https://ua.jooble.org/uk" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/jooble-logo2.png" /></a></div>
    <div><a href="https://eacea.ec.europa.eu/erasmus-plus_en" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/erasmus.png" /></a></div>
    <div><a href="http://webdesign.vntu.edu.ua/" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/webdes.png" /></a></div>
    <div><a href="https://mon.gov.ua/ua" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/mon.png" /></a></div>
    <div><a href="https://miratech.ua/" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/miratech.svg" /></a></div>
    <!--<div><a href="https://vntu.nuwork.me/" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/service-first-work-vntu-nuwork.png" /></a></div>-->
    <div><a href="https://flexsim.pl/" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/flexsim.png" /></a></div>
    <div><a href="http://pntl.edu.vn.ua/" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/pntl.png" /></a></div>
    <div><a href="http://www.campuschina.org/" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/china.png" /></a></div>
    <div><a href="http://pedpresa.ua/" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/pedrpesacom.png" /></a></div>
    <div><a href="http://vtl.in.ua/" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/vtl-logo.png" /></a></div>
    <div><a href="https://nkrzi.gov.ua/" target="_new"><img data-lazy="https://cdn.vntu.edu.ua/sites/contrib/banners/nkrzi.png" /></a></div>
</div>
</div>

  </div>


  </div>

  </div>
</div>

<section id="footer">
  <div class="container horzlimiter">
    <div class="psm-block">
        <div class="row text-left text-xs-center text-sm-left text-md-left">
            <div class="col-xs-12 col-sm-3 col-md-3">
                  <div class="region region-footer-first">
    <div class="block" role="navigation" aria-labelledby="block-footer-block4-menu" id="block-footer-block4">

  <h4 class="menu-theader" id="block-footer-block4-menu">Структура</h4>



      <ul class="menu menu--footer nav">
                      <li class="first">
                                                <a href="/uk/institutes-and-departments.html" >Факультети та кафедри</a>
              </li>
                      <li>
                                                <a href="http://sts.vntu.edu.ua/" >Студентське самоврядування</a>
              </li>
                      <li>
                                                <a href="/2016-11-18-11-17-05.html" >Конференція трудового колективу</a>
              </li>
                      <li>
                                                <a href="/uk/about-university/administration.html" >Керівництво університету</a>
              </li>
                      <li class="last">
                                                <a href="/lists/organizations" >Організації університету</a>
              </li>
        </ul>


  </div>

  </div>

            </div>
            <div class="col-xs-12 col-sm-3 col-md-3">
                  <div class="region region-footer-second">
    <div class="block" role="navigation" aria-labelledby="block-education-menu" id="block-education">

  <h4 class="menu-theader" id="block-education-menu">Освіта</h4>



      <ul class="menu menu--footer-2 nav">
                      <li class="first">
                                                <a href="/information-for-enrollee" >Приймальна комісія</a>
              </li>
                      <li>
                                                <a href="https://cde.vntu.edu.ua/" >Дистанційна освіта</a>
              </li>
                      <li>
                                                <a href="http://inmad.vntu.edu.ua/" >Аспірантура та докторантура</a>
              </li>
                      <li>
                                                <a href="http://indp.vntu.edu.ua/" >Доуніверситетська підготовка</a>
              </li>
                      <li class="last">
                                                <a href="/go/shedule" >Розклад занять</a>
              </li>
        </ul>


  </div>

  </div>

            </div>
            <div class="col-xs-12 col-sm-3 col-md-3">
                  <div class="region region-footer-third">
    <div class="block" role="navigation" aria-labelledby="block-footer3-menu" id="block-footer3">

  <h4 class="menu-theader" id="block-footer3-menu">Діяльність</h4>



      <ul class="menu menu--footer-3 nav">
                      <li class="first">
                                                <a href="/uk/research.html" >Наукова діяльність</a>
              </li>
                      <li>
                                                <a href="/uk/publishing-activities.html" >Видавнича діяльність</a>
              </li>
                      <li>
                                                <a href="/uk/international-activities.html" >Міжнародна діяльність</a>
              </li>
                      <li>
                                                <a href="/student-activities.html" >Студентська діяльність</a>
              </li>
                      <li class="last">
                                                <a href="https://soc.vntu.edu.ua/" >Організаційна діяльність</a>
              </li>
        </ul>


  </div>

  </div>

            </div>
            <div class="col-xs-12 col-sm-3 col-md-3">
                  <div class="region region-footer-fourth">
    <div class="block" role="navigation" aria-labelledby="block-footer4-menu" id="block-footer4">

  <h4 class="menu-theader" id="block-footer4-menu">Ресурси</h4>



      <ul class="menu menu--footer-4 nav">
                      <li class="first">
                                                <a href="https://iq.vntu.edu.ua/" >JetIQ</a>
              </li>
                      <li>
                                                <a href="http://lib.vntu.edu.ua/" >Бібліотека</a>
              </li>
                      <li>
                                                <a href="http://posibnyky.vntu.edu.ua/" >Електронні посібники</a>
              </li>
                      <li>
                                                <a href="https://ir.lib.vntu.edu.ua/" >Інституційний репозиторій</a>
              </li>
                      <li>
                                                <a href="http://conferences.vntu.edu.ua/" >Конференції ВНТУ</a>
              </li>
                      <li class="last">
                                                <a href="http://journals.vntu.edu.ua/" >Журнали ВНТУ</a>
              </li>
        </ul>


  </div>

  </div>

            </div>
        </div>
        <div class="row border-hr">&nbsp;</div>
    </div>

        <div class="row">
        <div class="col-xs-12 col-sm-12 col-md-12 mt-2 mt-sm-2 text-white twelve-text">
            <div class="float-right"><a href="https://ukc.gov.ua/" target="_new"><img id="ukc-banner" src="/style/ukc.png"></a></div>
            <p class="copy-width-lim">&copy; 2026 <span class="serp-text">Вінницький Національний Технічний Університет</span> <span class="serp-line">|</span> Адреса: <span class="serp-text">21021 Вінниця, Хмельницьке шосе 95</span></p>
            <p class="copy-width-lim">Електронна пошта: <a href="mailto:vntu@vntu.edu.ua">vntu@vntu.edu.ua</a> <span class="serp-line">|</span> Контактний телефон: <b>+38 (0432) 65-19-03</b> (канцелярія) <span class="serp-line">|</span>  Факс: <b>+38 (0432) 46-57-72</b><br>
            <br />
            Приймальна комісія: <b>+38 (0432) 46-66-67</b><br>
            Підготовче відділення: <b>+38 (0432) 43-34-15</b></p>
        </div>
    </div>
    <div class="row">
        <div class="col-xs-12 col-sm-12 col-md-12 mt-2 mt-sm-5 twelve-text">
            <a href="#" onclick="jump_top()">Наверх</a> <span class="serp-line">|</span> <a href="/go/maps" target="_new">Знайти на мапі</a> <span class="serp-line">|</span> <a href="/uk/privacy.html">Політика конфіденційності</a> <span class="serp-line">|</span> <a href="/uk/contacts.html">Зв&#039;язок із нами</a>
            <ul class="list-unstyled list-inline social soc-minify float-right">
                <li class="list-inline-item"><a href="/go/facebook" target="_blank"><i class="fa fa-facebook"></i></a></li>
                <li class="list-inline-item"><a href="/go/twitter" target="_blank"><i class="fa fa-twitter"></i></a></li>
                <li class="list-inline-item"><a href="/go/linkedin" target="_blank"><i class="fa fa-linkedin"></i></a></li>
                <li class="list-inline-item"><a href="/go/instagram" target="_blank"><i class="fa fa-instagram"></i></a></li>
                <li class="list-inline-item"><a href="/go/youtube" target="_blank"><i class="fa fa-youtube-play"></i></a></li>
                <li class="list-inline-item"><a href="/uk/contacts.html"><i class="fa fa-envelope"></i></a></li>
            </ul>
            <div></div>
        </div>
    </div>

  </div>
</section>

<div id="gdpr-banner" class="gdpr-consent-banner">
    <div class="gdpr-consent-banner-wrap">
        <p>Ми використовуємо cookies для поліпшення якості роботи веб-сайту та аналізу трафіку.</p>
        <div class="gdpr-consent-banner-button-wrap">
            <input type="hidden" id="d8_lang_id" name="d8_lang_id" value="uk" />
            <a class="btn btn-secondary" href="/uk/privacy.html" target="_blank">Деталі</a>
            <a class="btn btn-primary gdpr-accept-js" href="#" onclick="set_cookies_accept();">Приймаю</a>
        </div>
    </div>
</div>

  </div>


<script type="application/json" data-drupal-selector="drupal-settings-json">{"path":{"baseUrl":"\/","scriptPath":null,"pathPrefix":"","currentPath":"index.html","currentPathIsAdmin":false,"isFront":true,"currentLanguage":"uk"},"pluralDelimiter":"\u0003","suppressDeprecationErrors":true,"ajaxPageState":{"libraries":"bootstrap\/popover,bootstrap\/tooltip,system\/base,views\/views.module,views_infinite_scroll\/views-infinite-scroll","theme":"vntu","theme_token":null},"ajaxTrustedUrl":[],"bootstrap":{"forms_has_error_value_toggle":1,"modal_animation":1,"modal_backdrop":"true","modal_focus_input":1,"modal_keyboard":1,"modal_select_text":1,"modal_show":1,"modal_size":"","popover_enabled":1,"popover_animation":1,"popover_auto_close":1,"popover_container":"body","popover_content":"","popover_delay":"0","popover_html":0,"popover_placement":"right","popover_selector":"","popover_title":"","popover_trigger":"click","tooltip_enabled":1,"tooltip_animation":1,"tooltip_container":"body","tooltip_delay":"0","tooltip_html":0,"tooltip_placement":"auto left","tooltip_selector":"","tooltip_trigger":"hover"},"views":{"ajax_path":"\/views\/ajax","ajaxViews":{"views_dom_id:46779e5416b33edea3eb3552206bd5bd6a8835f06561138edf4350a937a3df04":{"view_name":"index","view_display_id":"vntu_news","view_args":"","view_path":"\/index.html","view_base_path":"index.html","view_dom_id":"46779e5416b33edea3eb3552206bd5bd6a8835f06561138edf4350a937a3df04","pager_element":0}}},"user":{"uid":0,"permissionsHash":"17e84872a20ea499e7c291b7b23ac125f10e1b734f43f4267a369beb15ef2fb2"}}</script>
<script src="/sites/default/files/js/js_0-4rEWL9cPOt52OwGOfdG_-7hkKR9CnlC04gSPIZ7Mw.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.4.1/js/bootstrap.min.js" integrity="sha512-oBTprMeNEKCnqfuqKd6sbvFzmFQtlXS3e0C/RGFV0hD6QzhHV+ODfaQbAlmY6/q0ubbwlAM/nCJjkrgA3waLzg==" crossorigin="anonymous" referrerpolicy="no-referrer"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/slick-carousel/1.8.0/slick.min.js" integrity="sha512-XtmMtDEcNz2j7ekrtHvOVR4iwwaD6o/FUJe6+Zq+HgcCsk3kj4uSQQR8weQ2QVj1o0Pk6PwYLohm206ZzNfubg==" crossorigin="anonymous" referrerpolicy="no-referrer"></script>
<script src="/style/js/vntu.js?ver=2022013001"></script>
<script src="/sites/default/files/js/js_Miyuolt_gGtyyEy9Mtyk2QmxrGlXCqEBvMif9jeLmtg.js"></script>

</body>
</html>
* Connection #0 to host vntu.edu.ua:443 left intact
```

---

### A.2. Запит без захисту з'єднання

**Команда:**

```
curl -v http://neverssl.com
```

**Вивід:**

```
* Host neverssl.com:80 was resolved.
* IPv6: (none)
* IPv4: 34.223.124.45
*   Trying 34.223.124.45:80...
* connect to 34.223.124.45 port 80 from 0.0.0.0 port 57540 failed: Timed out
* Failed to connect to neverssl.com:80 after 21062 ms: Could not connect to server
* closing connection #0
curl: (28) Failed to connect to neverssl.com:80 after 21062 ms: Could not connect to server
```

---

### A.3. Запит до служби доменних імен

**Команда:**

```
Resolve-DnsName vntu.edu.ua
```

**Вивід:**

```
Name                                           Type   TTL   Section    IPAddress
----                                           ----   ---   -------    ---------
vntu.edu.ua                                    A      164   Answer     194.146.143.10
```

**Команда:**

```
Resolve-DnsName vntu.edu.ua
```

**Вивід:**

```
Name                                           Type   TTL   Section    IPAddress
----                                           ----   ---   -------    ---------
vntu.edu.ua                                    A      600   Answer     194.146.143.10
```

**Зафіксовані значення:**

| Параметр | Перше виконання | Повторне виконання |
|---|---|---|
| Час виконання | 07:55 | 08:02 |
| IP-адреса | 194.146.143.10 | 194.146.143.10 |
| Значення TTL | 164 | 600 |

---

### A.4. Контрольний ресурс

**Команда:**

```
curl -v https://google.com
```

**Вивід:**

```
* Host google.com:443 was resolved.
* IPv6: (none)
* IPv4: 142.250.109.113, 142.250.109.100, 142.250.109.138, 142.250.109.102, 142.250.109.101, 142.250.109.139
*   Trying 142.250.109.113:443...
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* ALPN: server accepted http/1.1
* Established connection to google.com (142.250.109.113 port 443) from 192.168.0.102 port 49910
* using HTTP/1.x
> GET / HTTP/1.1
> Host: google.com
> User-Agent: curl/8.21.0
> Accept: */*
>
* Request completely sent off
* schannel: remote party requests renegotiation
* schannel: renegotiating SSL/TLS connection
* schannel: SSL/TLS connection renegotiated
< HTTP/1.1 301 Moved Permanently
< Location: https://www.google.com/
< Content-Type: text/html; charset=UTF-8
< Content-Security-Policy-Report-Only: object-src 'none';base-uri 'self';script-src 'nonce-GSu71IA8LYsVDJXPh2c9Ow' 'strict-dynamic' 'report-sample' 'unsafe-eval' 'unsafe-inline' https: http:;report-uri https://csp.withgoogle.com/csp/gws/other-hp
< Date: Tue, 15 Sep 2026 05:13:46 GMT
< Expires: Thu, 15 Oct 2026 05:13:46 GMT
< Cache-Control: public, max-age=2592000
< Server: gws
< Content-Length: 220
< X-XSS-Protection: 0
< X-Frame-Options: SAMEORIGIN
< Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000
<
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="https://www.google.com/">here</A>.
</BODY></HTML>
* Connection #0 to host google.com:443 left intact
```

---

### A.5. Ресурси з некоректною конфігурацією сертифіката

**Випадок 1**

```
curl -v https://expired.badssl.com
```

```
* Host expired.badssl.com:443 was resolved.
* IPv6: (none)
* IPv4: 104.154.89.105
*   Trying 104.154.89.105:443...
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* schannel: next InitializeSecurityContext failed: SEC_E_CERT_EXPIRED (0x80090328) - The received certificate has expired.
* closing connection #0
curl: (35) schannel: next InitializeSecurityContext failed: SEC_E_CERT_EXPIRED (0x80090328) - The received certificate has expired.
```

**Випадок 2**

```
curl -v https://wrong.host.badssl.com
```

```
*   Trying 104.154.89.105:443...
* Host wrong.host.badssl.com:443 was resolved.
* IPv6: (none)
* IPv4: 104.154.89.105
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* schannel: SNI or certificate check failed: SEC_E_WRONG_PRINCIPAL (0x80090322) - The target principal name is incorrect.
* closing connection #0
curl: (60) schannel: SNI or certificate check failed: SEC_E_WRONG_PRINCIPAL (0x80090322) - The target principal name is incorrect.
More details here: https://curl.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the webpage mentioned above.
```

**Випадок 3**

```
curl -v https://self-signed.badssl.com
```

```
*   Trying 104.154.89.105:443...
* Host self-signed.badssl.com:443 was resolved.
* IPv6: (none)
* IPv4: 104.154.89.105
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* schannel: SEC_E_UNTRUSTED_ROOT (0x80090325) - The certificate chain was issued by an authority that is not trusted.
* closing connection #0
curl: (60) schannel: SEC_E_UNTRUSTED_ROOT (0x80090325) - The certificate chain was issued by an authority that is not trusted.
More details here: https://curl.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the webpage mentioned above.
```

---

## Частина B. Власна модель рівнів

**Кількість виділених груп:** 5

| № | Назва групи | Рядки виводу, віднесені до групи | Обґрунтування |
|---|---|---|---|
| 1 | Запит до ресурсу | > GET / HTTP/1.1 <br>> Host: vntu.edu.ua<br>> User-Agent: curl/8.21.0<br>> Accept: \*/\* | Клієнт формує запит до вебресурсу. Найближчий до користувача, бо це результат, отриманий чітко через дії користувача. |
| 2 | Відповідь від сервера | < HTTP/1.1 200 OK<br>< Server: nginx<br>< Content-Type: text/html; charset=UTF-8<br>< Transfer-Encoding: chunked<br>< Connection: keep-alive<br>< Cache-Control: max-age=900, public<br>< Date: Tue, 15 Sep 2026 04:10:52 GMT<br>< X-UA-Compatible: IE=edge<br>< Content-language: uk<br>< X-Content-Type-Options: nosniff<br>< X-Frame-Options: SAMEORIGIN<br>< Permissions-Policy: interest-cohort=()<br>< Expires: Sun, 19 Nov 1978 05:00:00 GMT<br>< Last-Modified: Tue, 15 Sep 2026 04:10:51 GMT<br>< ETag: "1789445451"<br>< Vary: Cookie<br>< X-Powered-By: VNTU/JetIQ<br>< <br>\<!DOCTYPE html><br>...<br>\</html> | Сервер відповідає на запит з відповідними даними. Ближче, ніж група 3, оскільки це відповідь, яку очікує користувач. |
| 3 | Забезпечення безпеки | * schannel: disabled automatic use of client certificate<br>* schannel: remote party requests renegotiation<br>* schannel: renegotiating SSL/TLS connection<br>* schannel: SSL/TLS connection renegotiated<br>* schannel: remote party requests renegotiation<br>* schannel: renegotiating SSL/TLS connection<br>* schannel: SSL/TLS connection renegotiated | `schannel` є вбудованим постачальником послуг з безпеки Windows, що встановлює шифрування SSL/TLS. Нижче за групу 2, оскільки не ініціюється безпосередньо користувачем та є вбудованою функцією ОС. |
| 4 | Вибір протоколу обміну | * ALPN: curl offers http/1.1<br>* ALPN: server accepted http/1.1 | Розширення протоколу TLS, що встановлює використаний протокол для передачі даних. Я вважаю, що дана група ближче до АЗ за групу 3, оскільки перед встановленням захищеного зв'язку клієнту потрібно домовитися з сервером про використаний протокол. |
| 5 | Встановлення з'єднання | *   Trying 194.146.143.10:443...<br>* Established connection to vntu.edu.ua (194.146.143.10 port 443) from 192.168.0.102 port 50125<br>* using HTTP/1.x | Встановлення з'єднання клієнт-сервер. Найнижчий рівень, оскільки перед будь-якими діями потрібне встановлення базового з'єднання, як спроба зв'язатися з певним сервером. |

**Рядки, які не вдалося віднести до жодної групи:**

| Рядок виводу | Причина утруднення |
|---|---|
| * Host vntu.edu.ua:443 was resolved.<br>* IPv6: (none)<br>* IPv4: 194.146.143.10 | Для моделі рівнів потрібно було виділити етапи взаємодії клієнт-сервер. Клієнт отримує дані про домен від специфічного серверу, однак у всіх інших групах клієнт взаємодіє з потрібним сервером. Даний етап можна віднести до КУДАТО, бо ШОТО. |
| * Connection #0 to host vntu.edu.ua:443 left intact | Констатанція факту стану з'єднання. |
| * Request completely sent off | Повідомлення користувачеві про завершення передачі запиту з боку клієнту. |

---

## Контрольні питання

**1. Скільки рядків діагностичного виводу передує отриманню даних сторінки (завдання A.1)?**

> 16 рядків діагностичного виводу.

**2. Які рядки наявні у виводі A.1 і відсутні у виводі A.2? Чим це зумовлено?**

> Відсутні рядки `schannel` i `ALPN`, оскільки безпечний зв'язок не було встановлено. Ресурс А.2 не використовує захищений протокол. Також відсутні рядки даних сайту запиту, оскільки клієнт не зміг зв'язатися з сервером.

**3. Звідки у виводі з'явилося значення `443`, якщо його не було вказано в адресі?**

> Якщо TCP порт не вказано користувачем, він буде автоматично обраний клієнтом, в залежності від серверу, до якого звертаються. 443 відповідає `https`.

**4. Як змінилося значення TTL між двома запитами (A.3)? Що означає це число?**

> TTL (Time to Live, час життя) є значенням DNS, що відповідає за час, протягом якого запис DNS зберігається. Значення TTL повторного виконання доволі вище за перше виконання. Воно зазвичай менше, але в моєму випадку воно вище. Це зумовлено оновленням кешу, тобто через 164 секунди DNS оновився та, можливо, сервер оновлюється кожні 15 хвилин. 

**5. Чим відрізняються між собою три причини помилок із завдання A.5? Сформулювати кожну однією фразою.**

| Випадок | Причина недовіри |
|---|---|
| `expired` | Термін сертифіката закінчився |
| `wrong.host` | Сертифікат належить іншому домену |
| `self-signed` | Сертифікат підписаний власником домену або приватною організацією, якій не довіряють |

**6. Три рядки з власних виводів, про які не йшлося на лекції 1:**

| № | Рядок виводу | Джерело (номер завдання) |
|---|---|---|
| 1 | < X-Content-Type-Options: nosniff | А.1 |
| 2 | < ETag: "1789445451" | A.1 |
| 3 | < X-Powered-By: VNTU/JetIQ | A.1 |

---

## Висновки

**D.1. Що виявилося неочевидним або несподіваним**

> Подвійне
> `* schannel: remote party requests renegotiation`<br>
> `* schannel: renegotiating SSL/TLS connection`<br>
> `* schannel: SSL/TLS connection renegotiated`.<br>
> Несподівано та незрозуміло, навіщо повторно встановлювати захищене з'єднання. Також здивувало, наскільки маленькою була відповідь команди Resolve-DnsName. Приклади, які видає браузер, мають або декілька записів в самій таблиці, або детальніші пояснення після таблиці.

**D.2. Чому саме така кількість груп у частині B**

> Спочатку груп було вісім. Результат команди А.1 було поділено на частини, які можна було візуально побачити. Далі групи було розподілено та об'єднано - груп стало п'ять, з результатом `<!DOCTYPE html>...</html>` як найближчим до користувача, оскільки, на мою минулу думку, користувач використовує команду `curl` щоб отримати дані про вебсайт. На мою думку зараз, `curl -v` використовується для спостереження взаємодій клієнт-сервер та найближчим рівнем до користувача буде саме команда, що користувач ініціює. Груп було б чотири, але я вирішила винести `ALPN` поза групи встановлення з'єднання. Я також вважаю, що груп може бути більше, або деякі рядки можна винести поза моделі, але, нажаль, моє розуміння та вміння читати відповідь команди не високе.

**D.3. Питання, яке залишилося без відповіді**

> Чому cmd не реагує на команду Resolve-DnsName. Чому в А.1 відбулося повторне встановлення захищеного з'єднання. Як саме працює `curl -v https://<домен> > raw/a1-curl-https.txt 2>&1` та чому команда не спрацювала. Які саме рядки діагностичні, всі, чи лише ті, позначені "*". 

---

## Використання штучного інтелекту

**Факт використання:** не використано

**Установлений рівень для цієї роботи:** Р3 — ШІ як співвиконавець

**Фактичний рівень використання:** Р1

### Підтвердження

Підтверджую, що всі наведені в цьому звіті виводи команд отримано мною особисто внаслідок фактичного виконання відповідних дій, а відомості цього розділу є повними та достовірними.

---

## Примітки виконавця

> Спершу D.3 також мало питання "Чому в А.2 не вдалося підключитися до серверу ресурсу, коли при введенні тієї ж команди в cmd з правами адміністратора з'єднання було успішним.". Після відкриття ресурсу в браузері, я дізналася, що з правами адміністратора система "довірилась" моїм діям та дозволила мені отримати інформацію про сайт, що не має захищення.
> Також інтересно, чому деякі команди працюють в cmd, а деякі лише в pwsh. В практичній роботі нема чіткого пояснення, яку програму потрібно використати, а зміст лекції містить команди, виконанні саме в cmd. Лише сподіваюся, що завдання було виконане правильно.
