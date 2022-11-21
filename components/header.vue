<template>
  <header class="p-3 mb-3 border-bottom">
    <div class="container">
      <div
        class="
          d-flex
          flex-wrap
          align-items-center
          justify-content-center justify-content-lg-start
        "
      >
        <a
          href="/"
          class="
            d-flex
            align-items-center
            mb-2 mb-lg-0
            text-dark text-decoration-none
          "
        >
          <img src="../static/logo.png" alt="logo" width="60" height="40" />
        </a>

        <ul
          class="
            nav
            col-12 col-lg-auto
            me-lg-auto
            mb-2
            justify-content-center
            mb-md-0
          "
        >
          <li><a href="/" class="nav-link px-4 link-dark">Home</a></li>
          <li><a href="/explore" class="nav-link px-4 link-dark">Explore</a></li>
          <!-- <li><a href="defi" class="nav-link px-4 link-dark">DeFi</a></li> -->
          <!-- <li><a href="nft" class="nav-link px-4 link-dark">NFT</a></li> -->
        </ul>

        <form class="col-12 col-lg-auto mb-3 mb-lg-0 me-lg-3" role="search">
          <input
            type="search"
            class="form-control"
            placeholder="Search more ..."
            aria-label="Search"
          />
        </form>

        <!-- <div class="text-end">
          <a href="/login">
            <button type="button" class="btn btn-light text-dark me-2">
              登录
            </button>
          </a>
          <a href="/register">
            <button type="button" class="btn btn-primary">注册</button>
          </a>
        </div> -->

        <!-- <div class="dropdown text-end">
                    <a href="#" class="d-block link-dark text-decoration-none dropdown-toggle" data-bs-toggle="dropdown"
                        aria-expanded="false">
                        <img src="https://github.com/mdo.png" alt="mdo" width="32" height="32" class="rounded-circle">
                    </a>
                    <ul class="dropdown-menu text-small" style="">
                        <li><a class="dropdown-item" href="#">New project...</a></li>
                        <li><a class="dropdown-item" href="#">Settings</a></li>
                        <li><a class="dropdown-item" href="#">Profile</a></li>
                        <li>
                            <hr class="dropdown-dider">
                        </li>
                        <li><a class="dropdown-item" href="#">Sign out</a></li>
                    </ul>
                </div> -->
      </div>
    </div>
  </header>
</template>