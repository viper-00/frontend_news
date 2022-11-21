<template>
    <div class="text-center" id="app">
    <main class="form-signin w-100 m-auto">
        <form>
            <img class="mb-4" src="/docs/5.2/assets/brand/bootstrap-logo.svg" alt="" width="72" height="57">
            <h1 class="h3 mb-3 fw-normal">登录</h1>

            <div class="form-floating">
                <input type="email" class="form-control" id="floatingInput" placeholder="用户名/邮箱">
                <label for="floatingInput">用户名/邮箱</label>
            </div>
            <div class="form-floating">
                <input type="password" class="form-control" id="floatingPassword" placeholder="密码">
                <label for="floatingPassword">密码</label>
            </div>

            <div class="checkbox mb-3">
                <label>
                    <input type="checkbox" value="remember-me"> 记住我
                </label>
            </div>
            <button class="w-100 btn btn-lg btn-primary" type="submit">登录</button>
            <p class="mt-5 mb-3 text-muted">©2022</p>
        </form>
    </main>
</div>
</template>

<script>
export default {
    name: "loginPage"
}
</script>

<style>
    /* html, body, #app {
        height: 100%;
    } */

    #app {
        height: 100%;
        display: flex;
        align-items: center;
        padding-top: 40px;
        padding-bottom: 40px;
        background-color: #f5f5f5;
    }

    .form-signin {
        max-width: 330px;
        padding: 15px;
    }
</style>