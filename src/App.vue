<template>
  <div id="app">
    <nav class="navbar navbar-expand-md">
      <a class="navbar-brand" href="#">MyProject</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarsExampleDefault" aria-controls="navbarsExampleDefault" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarsExampleDefault">
      <ul class="navbar-nav mr-auto">
       <li class="nav-item active">
          <a class="nav-link" href="#">Home</a>
        </li>
        <li class="nav-item">
         <a class="nav-link" href="#">Link1</a>
        </li>
        <li class="nav-item">
         <a class="nav-link" href="#">Link2</a>
        </li>
      </ul>

      <ul class="navbar-nav navbar-right">
        <li class="nav-item active">
          <a class="nav-link" href="#">Welcome</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Logout</a>
        </li>
      </ul>

      </div>
    </nav>

    <header id="header">
      <div class="container">
        <div class="row">
          <div class="col-md-10">
            <h1><span class="fas fa-chart-line"></span> Dashboard <small>manage your site</small></h1>
          </div>
          <div class="col-md-2">
              <div class="dropdown create">
                <a class="btn btn-secondary dropdown-toggle" href="#" role="button" id="dropdownMenuLink" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                  Drop Down
                </a>

                <div class="dropdown-menu" aria-labelledby="dropdownMenuLink">
                  <a class="dropdown-item" href="#">Action #1</a>
                  <a class="dropdown-item" href="#">Action #2</a>
                  <a class="dropdown-item" href="#">Action #3</a>
                </div>
            </div>

          </div>
        </div>
      </div>
    </header>
    <section id="breadcrumb">
      <div class="container">
        <ol class="breadcrumb">
          <li class="active">Dashboard</li>
        </ol>
      </div>
    </section>
    <section id="main">
      <div class="container">
        <div class="row">
          <div class="col-md-3">
            <div class="list-group">
              <button type="button" class="list-group-item list-group-item-action active main-color-bg">
                <span class="fas fa-cogs"></span>  Dashboard
              </button>
              <button type="button" class="list-group-item list-group-item-action"><span class="fas fa-dot-circle"></span>  item #1</button>
              <button type="button" class="list-group-item list-group-item-action"><span class="fas fa-dot-circle"></span>  item #2</button>
              <button type="button" class="list-group-item list-group-item-action"><span class="fas fa-dot-circle"></span>  item #3</button>
              <button type="button" class="list-group-item list-group-item-action" disabled><span class="fas fa-dot-circle"></span>  item #4</button>
            </div>
          </div>
          <div class="col-md-9">

          </div>
        </div>
      </div>
    </section>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
body {
  background: #f4f4f4;
}
/* NavBar */
.navbar {
  min-height: 33px !important;
  margin-bottom: 0;
  border-radius: 0;
}

.navbar-nav > li > a, .navbar-brand {
  padding-top: 6px !important;
  padding-bottom: 0 !important;
  height: 33px;
}

.navbar {
  background-color: #245999;
}
.navbar .navbar-brand {
  color: #d1eef5;
}
.navbar .navbar-brand:hover,
.navbar .navbar-brand:focus {
  color: #a1fff0;
}
.navbar .navbar-text {
  color: #d1eef5;
}
.navbar .navbar-text a {
  color: #a1fff0;
}
.navbar .navbar-text a:hover,
.navbar .navbar-text a:focus {
  color: #a1fff0;
}
.navbar .navbar-nav .nav-link {
  color: #d1eef5;
  border-radius: .25rem;
  margin: 0 0.25em;
}
.navbar .navbar-nav .nav-link:not(.disabled):hover,
.navbar .navbar-nav .nav-link:not(.disabled):focus {
  color: #a1fff0;
}
.navbar .navbar-nav .dropdown-menu {
  background-color: #245999;
  border-color: #689efe;
}
.navbar .navbar-nav .dropdown-menu .dropdown-item {
  color: #d1eef5;
}
.navbar .navbar-nav .dropdown-menu .dropdown-item:hover,
.navbar .navbar-nav .dropdown-menu .dropdown-item:focus,
.navbar .navbar-nav .dropdown-menu .dropdown-item.active {
  color: #a1fff0;
  background-color: #689efe;
}
.navbar .navbar-nav .dropdown-menu .dropdown-divider {
  border-top-color: #689efe;
}
.navbar .navbar-nav .nav-item.active .nav-link,
.navbar .navbar-nav .nav-item.active .nav-link:hover,
.navbar .navbar-nav .nav-item.active .nav-link:focus,
.navbar .navbar-nav .nav-item.show .nav-link,
.navbar .navbar-nav .nav-item.show .nav-link:hover,
.navbar .navbar-nav .nav-item.show .nav-link:focus {
  color: #a1fff0;
  background-color: #689efe;
}
.navbar .navbar-toggle {
  border-color: #689efe;
}
.navbar .navbar-toggle:hover,
.navbar .navbar-toggle:focus {
  background-color: #689efe;
}
.navbar .navbar-toggle .navbar-toggler-icon {
  color: #d1eef5;
}
.navbar .navbar-collapse,
.navbar .navbar-form {
  border-color: #d1eef5;
}
.navbar .navbar-link {
  color: #d1eef5;
}
.navbar .navbar-link:hover {
  color: #a1fff0;
}

/* Custom */
.main-color-bg {
  background-color: #245999 !important;
  border-color: #689efe !important;
  color: #ffffff;
}

/* Header */
#header {
  background: #333333;
  color: #ffffff;
  padding-bottom: 10px;
  margin-bottom: 15px;
}
#header .create {
  padding-top: 20px;
}

/* Breadcrumb */
.breadcrumb {
  background: #cccccc;
  color: #333333;
}
.breadcrumb a {
  color:#333333;
}

@media (max-width: 575px) {
  .navbar-expand-sm .navbar-nav .show .dropdown-menu .dropdown-item {
    color: #d1eef5;
  }
  .navbar-expand-sm .navbar-nav .show .dropdown-menu .dropdown-item:hover,
  .navbar-expand-sm .navbar-nav .show .dropdown-menu .dropdown-item:focus {
    color: #a1fff0;
  }
  .navbar-expand-sm .navbar-nav .show .dropdown-menu .dropdown-item.active {
    color: #a1fff0;
    background-color: #689efe;
  }
}

@media (max-width: 767px) {
  .navbar-expand-md .navbar-nav .show .dropdown-menu .dropdown-item {
    color: #d1eef5;
  }
  .navbar-expand-md .navbar-nav .show .dropdown-menu .dropdown-item:hover,
  .navbar-expand-md .navbar-nav .show .dropdown-menu .dropdown-item:focus {
    color: #a1fff0;
  }
  .navbar-expand-md .navbar-nav .show .dropdown-menu .dropdown-item.active {
    color: #a1fff0;
    background-color: #689efe;
  }
}

@media (max-width: 991px) {
  .navbar-expand-lg .navbar-nav .show .dropdown-menu .dropdown-item {
    color: #d1eef5;
  }
  .navbar-expand-lg .navbar-nav .show .dropdown-menu .dropdown-item:hover,
  .navbar-expand-lg .navbar-nav .show .dropdown-menu .dropdown-item:focus {
    color: #a1fff0;
  }
  .navbar-expand-lg .navbar-nav .show .dropdown-menu .dropdown-item.active {
    color: #a1fff0;
    background-color: #689efe;
  }
}

@media (max-width: 1199px) {
  .navbar-expand-xl .navbar-nav .show .dropdown-menu .dropdown-item {
    color: #d1eef5;
  }
  .navbar-expand-xl .navbar-nav .show .dropdown-menu .dropdown-item:hover,
  .navbar-expand-xl .navbar-nav .show .dropdown-menu .dropdown-item:focus {
    color: #a1fff0;
  }
  .navbar-expand-xl .navbar-nav .show .dropdown-menu .dropdown-item.active {
    color: #a1fff0;
    background-color: #689efe;
  }
}

.navbar-expand .navbar-nav .show .dropdown-menu .dropdown-item {
  color: #d1eef5;
}
.navbar-expand .navbar-nav .show .dropdown-menu .dropdown-item:hover,
.navbar-expand .navbar-nav .show .dropdown-menu .dropdown-item:focus {
  color: #a1fff0;
}
.navbar-expand .navbar-nav .show .dropdown-menu .dropdown-item.active {
  color: #a1fff0;
  background-color: #689efe;
}
</style>
