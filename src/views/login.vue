<template>
	<el-row>
		<el-col>
			<h2>随便放个登录用着先</h2>
		</el-col>
		<el-col :xs="{span:20,offset:2}" :sm="{span:10,offset:9}" :md="{span:8,offset:8}" :lg="{span:6,offset:9}">
			<el-form label-position="left" :model="loginForm" :rules="rules" ref="loginForm">
				<el-form-item label="用户名" prop="username">
					<el-input v-model="loginForm.username" />
				</el-form-item>
				<el-form-item label="密码" prop="password">
					<el-input v-model="loginForm.password" type="password" />
				</el-form-item>
				<el-form-item>
					<el-button @click="submit('loginForm')">登录</el-button>
					<el-button disabled>注册</el-button>
				</el-form-item>
			</el-form>
		</el-col>
	</el-row>
</template>

<script>
	export default {
		data() {
			return {
				loginForm: {
					username: "",
					password: "",
					returnUrl: ""
				},
				rules: {
					username: [{
						required: true,
						message: "用户名不能为空",
						trigger: "blur"
					}],
					password: [{
						required: true,
						message: "密码不能为空",
						trigger: "blur"
					}]
				}
			};
		},
		methods: {
			submit(refName) {
				this.$refs[refName].validate((valid) => {
					if (valid) {
						this.login();
					} else {
						return false;
					}
				});
			},
			login() {
				this.$axios
					.get("/sso/login", {
						params: this.loginForm
					})
					.then(res => {
						if (res.data.data) {
							window.location.href = res.data.data;
						} else {
							alert("用户名或密码错误！");
						}
					})
					.catch(function(res) {
						alert("服务器异常！" + res);
					});
			}
		},
		mounted() {
			this.loginForm.returnUrl = this.$route.query.returnUrl || "http://www.arkfancy.com";
		}
	}
</script>

<style>
</style>
