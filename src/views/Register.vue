<template>
  <div>
    <h1>农业一体化管理系统 - 注册</h1>
    <el-form ref="registerForm" :model="registerUser" :rules="registerRules" label-width="120px">
      <el-form-item label="用户名" prop="username">
        <el-input v-model="registerUser.username"></el-input>
      </el-form-item>
      <el-form-item label="真实姓名" prop="realName">
        <el-input v-model="registerUser.realName"></el-input>
      </el-form-item>
      <el-form-item label="手机号" prop="phone">
        <el-input v-model="registerUser.phone"></el-input>
      </el-form-item>
      <el-form-item label="性别" prop="gender">
        <el-radio-group v-model="registerUser.gender">
          <el-radio label="男">男</el-radio>
          <el-radio label="女">女</el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="邮箱" prop="email">
        <el-input v-model="registerUser.email"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input v-model="registerUser.password" type="password"></el-input>
      </el-form-item>
      <el-form-item label="确认密码" prop="confirmPassword">
        <el-input v-model="registerUser.confirmPassword" type="password"></el-input>
      </el-form-item>
      <el-form-item label="头像">
        <el-upload
            class="avatar-uploader"
            action="#"
            :show-file-list="false"
            :before-upload="beforeAvatarUpload"
        >
          <img v-if="registerUser.avatarUrl" :src="registerUser.avatarUrl" class="avatar" />
          <i v-else class="el-icon-plus avatar-uploader-icon"></i>
        </el-upload>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="handleRegister">注册</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue';
import { post } from '@/common'; // 假设这是请求方法的导入路径

const registerForm = ref(null);
const registerUser = reactive({
  username: '',
  realName: '',
  phone: '',
  gender: '男',
  email: '',
  password: '',
  confirmPassword: '',
  avatarUrl: ''
});

const registerRules = reactive({
  username: [
    { required: true, message: '请输入用户名', trigger: 'blur' }
  ],
  realName: [
    { required: true, message: '请输入真实姓名', trigger: 'blur' }
  ],
  phone: [
    { required: true, message: '请输入手机号', trigger: 'blur' },
    { pattern: /^1[3-9]\d{9}$/, message: '请输入正确的手机号', trigger: 'blur' }
  ],
  email: [
    { required: true, message: '请输入邮箱', trigger: 'blur' },
    { type: 'email', message: '请输入正确的邮箱地址', trigger: 'blur' }
  ],
  password: [
    { required: true, message: '请输入密码', trigger: 'blur' },
    { min: 6, message: '密码长度不能少于 6 位', trigger: 'blur' }
  ],
  confirmPassword: [
    { required: true, message: '请确认密码', trigger: 'blur' },
    {
      validator: (rule, value, callback) => {
        if (value !== registerUser.password) {
          callback(new Error('两次输入的密码不一致'));
        } else {
          callback();
        }
      },
      trigger: 'blur'
    }
  ]
});

const beforeAvatarUpload = (file) => {
  // 这里可以添加头像上传前的校验逻辑，例如文件类型、大小等
  console.log('上传头像前的处理', file);
  return true;
};

const handleRegister = () => {
  registerForm.value.validate((valid) => {
    if (valid) {
      post('/register', registerUser).then((result) => {
        alert('注册成功');
        // 可以跳转到登录页面
      }).catch((error) => {
        alert('注册失败：' + error.message);
      });
    }
  });
};
</script>

<style scoped>
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
</style>