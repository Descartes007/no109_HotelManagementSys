<template>
  <div class="login-container">
    <div class="bac"></div> <!-- 背景图层 -->
    <mu-card class="login-card" raised>
      <mu-form ref="form" :model="validateForm" class="mu-demo-form">
<!--        <mu-card-title sub-title="酒店管理系统-用户登录" class="title"></mu-card-title>-->
        <mu-card-text>
          <mu-form-item label="用户名" prop="username" :rules="usernameRules">
            <mu-text-field v-model="validateForm.username" outlined></mu-text-field>
          </mu-form-item>
          <mu-form-item label="密码" prop="password" :rules="passwordRules">
            <mu-text-field type="password" v-model="validateForm.password" outlined></mu-text-field>
          </mu-form-item>
          <div class="login-buttons">
            <mu-button color="primary" @click="submit">登录</mu-button>
            <mu-button color="secondary" @click="navigateTo('/')">先随便逛逛</mu-button>
          </div>
        </mu-card-text>
      </mu-form>
    </mu-card>
  </div>
</template>

<script>
import { userLogin } from "@/api/user";
import Cookies from 'js-cookie';

export default {
  name: "Login",
  data() {
    return {
      usernameRules: [
        { validate: (val) => !!val, message: '必须填写用户名' },
      ],
      passwordRules: [
        { validate: (val) => !!val, message: '必须填写密码' },
      ],
      validateForm: {
        username: '',
        password: ''
      }
    };
  },
  methods: {
    navigateTo(val) {
      this.$router.push(val);
    },
    submit() {
      this.$refs.form.validate().then((result) => {
        if (result == false) {
          this.$toast.warning("请填入账户密码！");
          return;
        } else {
          this.login();
        }
      });
    },
    login() {
      userLogin(this.validateForm.username, this.validateForm.password).then(res => {
        var response = res;
        if (response.data !== null) {
          Cookies.set('username', this.validateForm.username);
          Cookies.set('session', response.data.sessionId);
          Cookies.set('user_id', response.data.userId);
          this.navigateTo('/');
        } else if (response.code !== 1000) {
          this.$toast.error(response.message);
        }
      }).catch(err => {
        console.log(err);
      });
    }
  }
};
</script>

<style scoped>
/* 背景图层 */
.bac {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  background-image: url('../assets/background.jpg');
  background-size: cover;
  background-position: center center;
  z-index: -1;
  filter: blur(8px); /* 背景虚化处理 */
}


/* 登录容器样式 */
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  position: relative;
  padding: 0 20px;
}

/* 表单标题 */
.title {
  font-size: 36px; /* 放大字体 */
  font-weight: 600;
  text-align: center;
  margin-bottom: 30px;
  color: #333;
  letter-spacing: 1px;
}

/* 美化登录框 */
.login-card {
  width: 100%;
  max-width: 420px;
  padding: 40px;
  border-radius: 15px;
  background-color: rgba(255, 255, 255, 0.9); /* 背景透明度稍微降低 */
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
  backdrop-filter: blur(10px); /* 模糊背景 */
}


/* 每个表单项的样式 */
.mu-form-item {
  margin-bottom: 24px;
}

/* 输入框样式 */
.mu-text-field {
  width: 100%;
  border-radius: 10px;
  background-color: #f5f7fa;
  box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.1);
  font-size: 16px;
}

.mu-text-field .mu-input {
  padding: 14px 16px;
  border-radius: 10px;
}

.mu-text-field .mu-input:focus {
  border-color: #007bff;
  box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
}

/* 登录按钮容器 */
.login-buttons {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 20px;
}

/* 按钮样式 */
.mu-button {
  width: 48%;
  border-radius: 30px;
  font-weight: 500;
  padding: 12px 18px;
  transition: background-color 0.3s, transform 0.3s ease;
}

.mu-button[color="primary"] {
  background: linear-gradient(45deg, #007bff, #00d2ff);
  color: #fff;
}

.mu-button[color="primary"]:hover {
  background: linear-gradient(45deg, #0056b3, #007bff);
  transform: translateY(-3px);
}

.mu-button[color="secondary"] {
  background: linear-gradient(45deg, #6c757d, #a0a0a0);
  color: #fff;
}

.mu-button[color="secondary"]:hover {
  background: linear-gradient(45deg, #5a6268, #6c757d);
  transform: translateY(-3px);
}

.mu-button:active {
  transform: translateY(0);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}


</style>
