<template>
    <div class="row row-cols-1 row-cols-lg-2 row-cols-xl-3">
        <div class="col mx-auto">
            <div class="card mt-5">
                <div class="card-body">
                    <div class="border p-4 rounded">
                        <div class="text-center">
                            <h3 class="">Sign in</h3>
                            <router-link to="/khach-hang/dang-ky">
                                <p>Don't have an account yet? <a href="/khach-hang/dang-ky">Sign up here</a>
                                </p>
                            </router-link>
                        </div>
                        <div class="form-body">
                            <form class="row g-3">
                                <div class="col-12">
                                    <label class="form-label">Email Address</label>
                                    <input v-model="login.email" type="email" class="form-control"
                                        id="inputEmailAddress" placeholder="Email Address">
                                </div>
                                <div class="col-12">
                                    <label for="inputChoosePassword" class="form-label">Enter Password</label>
                                    <input v-model="login.password" type="password" class="form-control border-end-0"
                                        placeholder="Enter Password">
                                </div>
                                <div class="col-md-6">
                                    <div class="form-check form-switch">
                                        <input class="form-check-input" type="checkbox" id="flexSwitchCheckChecked"
                                            checked>
                                        <label class="form-check-label" for="flexSwitchCheckChecked">Remember Me</label>
                                    </div>
                                </div>
                                <div class="col-md-6 text-end">
                                    <router-link to="/quen-mat-khau">
                                        <a href="/quen-mat-khau">Forgot
                                            Password ?</a>
                                    </router-link>
                                </div>
                                <div class="col-12">
                                    <div class="d-grid">
                                        <button v-on:click="dangNhap()" type="button" class="btn btn-primary"><i
                                                class="bx bxs-lock-open"></i>Sign in</button>
                                    </div>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';
import { createToaster } from "@meforma/vue-toaster";
const toaster = createToaster({ position: "top-right" });
export default {
    data() {
        return {
            login: {},
        }
    },
    mounted() {
        this.checkLogin();
    },
    methods: {
        dangNhap() {
            axios
                .post("http://127.0.0.1:8000/api/khach-hang/dang-nhap", this.login)
                .then((res) => {
                    if (res.data.status) {
                        toaster.success(res.data.message)
                        localStorage.setItem('token_khachhang', res.data.token);
                        localStorage.setItem('ho_ten', res.data.ho_ten);
                        this.$router.push('/');
                    }
                    else {
                        toaster.error(res.data.message)
                    }
                })
                ;
        },
        checkLogin() {
            axios
                .get('http://127.0.0.1:8000/api/kiem-tra-token-khach-hang', {
                    headers: {
                        Authorization: 'Bearer ' + localStorage.getItem("token_khachhang")
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.$router.push('/');
                    }
                });
        }
    },

}
</script>
<style></style>
