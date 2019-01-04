<template>
  <div>
    <div class="crumbs crumbs-register">
      <el-breadcrumb separator="/" class="register-title">
        <el-breadcrumb-item><i class="el-icon-setting"></i>注册</el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="userContent">
      <el-form ref="form" :model="form" :rules="rules" label-width="80px">
        <el-form-item prop="username" label="用户名称">
          <el-input v-model="form.username" placeholder="请输入用户名称"></el-input>
        </el-form-item>
        <el-form-item prop="name" label="真实姓名">
          <el-input v-model="form.name" placeholder="请输入姓名"></el-input>
        </el-form-item>
        <el-form-item prop="password" label="密码">
          <el-input v-model="form.password" type="password" placeholder="请输入密码"></el-input>
        </el-form-item>
        <el-form-item prop="checkPass" label="确认密码">
          <el-input v-model="form.checkPass" type="password" placeholder="请再次输入密码"></el-input>
        </el-form-item>
        <el-form-item prop="email" label="邮箱">
          <el-input v-model="form.email" placeholder="请输入邮箱"></el-input>
        </el-form-item>
        <el-form-item prop="phone" label="手机">
          <el-input v-model="form.phone" placeholder="请输入手机号"></el-input>
        </el-form-item>
        <el-form-item prop="card" label="身份证">
          <el-input v-model="form.card" placeholder="请输入身份证号"></el-input>
        </el-form-item>
        <el-form-item prop="birth" label="出生日期">
          <el-col :span="24">
            <el-date-picker type="date" placeholder="选择日期" v-model="form.birth" style="width: 100%"></el-date-picker>
          </el-col>
        </el-form-item>
        <el-form-item prop="sex" label="性别">
          <el-select class="select-sex" v-model="form.sex" placeholder="请选择性别">
            <el-option label="男" value="man"></el-option>
            <el-option label="女" value="woman"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item prop="address" label="家庭住址">
          <el-col :span="24">
            <el-input v-model="form.address" placeholder="请输入地址"></el-input>
          </el-col>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit('form')">确定</el-button>
          <el-button @click="onCancle()">取消</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import Util from '../../utils/utils'
export default {
  name: 'Register',
  data () {
    let validatePass = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入密码'))
      } else {
        if (this.form.checkPass !== '') {
          this.$refs.form.validateField('checkPass')
        }
        callback()
      }
    }
    let validatePass2 = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请再次输入密码'))
      } else if (value !== this.form.password) {
        callback(new Error('两次输入的密码不一致'))
      } else {
        callback()
      }
    }
    let validateEmail = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入邮箱'))
      } else if (!Util.emailReg.test(this.form.email)) {
        callback(new Error('请输入正确的邮箱'))
      } else {
        callback()
      }
    }
    let validatePhone = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入手机号'))
      } else if (!Util.phoneReg.test(this.form.phone)) {
        callback(new Error('请输入正确的手机号'))
      } else {
        callback()
      }
    }
    let validateCard = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入身份证号'))
      } else if (!Util.idCardReg.test(this.form.card)) {
        callback(new Error('请输入正确的身份证号'))
      } else {
        callback()
      }
    }
    return {
      form: {
        username: 'admin',
        name: '黄阿龙',
        password: 'admin',
        checkPass: 'admin',
        email: '1593370493@qq.com',
        phone: '13615651578',
        card: '340621199407142830',
        birth: '1994-01-17',
        sex: '男',
        address: '合肥市'
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' }
        ],
        name: [
          { required: true, message: '请输入真实', trigger: 'blur' }
        ],
        password: [
          { validator: validatePass, trigger: 'blur' }
        ],
        checkPass: [
          { validator: validatePass2, trigger: 'blur' }
        ],
        email: [
          { validator: validateEmail, trigger: 'blur' }
        ],
        phone: [
          { validator: validatePhone, trigger: 'blur' }
        ],
        card: [
          { validator: validateCard, trigger: 'blur' }
        ],
        birth: [
          { required: true, message: '请输入出生日期', type: 'date', trigger: 'change' }
        ],
        sex: [
          { required: true, message: '请输入性别', trigger: 'blur' }
        ],
        address: [
          { required: true, message: '请输入地址', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    onSubmit (formName) {
      const self = this
      self.$refs[formName].validate((valid) => {
        if (valid) {
          self.$axios.put('http://localhost:3000/story/user/', JSON.stringify(self.form)).then(function (response) {
            let res = response.data
            if (res.errorCode === '0x0000') {
              self.$router.push('/register-success')
            } else {
              self.$notify({
                title: '提示',
                message: self.$createElemen('i', {style: 'color: red'}, '注册失败！')
              })
            }
          }).then(function (error) {
            console.log(error)
          })
        } else {
          return false
        }
      })
    },
    onCancle () {
      this.$router.push('/login')
    }
  }
}
</script>

<style scoped>
  .crumbs-register {
    background-color: #324157;
    height: 50px;
    line-height: 50px;
  }
  .register-title {
    line-height: 50px;
    margin: 0 auto;
    width: 50px;
    font-size: 16px;
  }
  .userContent {
    width: 400px;
    margin: 0 auto;
    text-align: center;
  }
  .select-sex {
    width: 320px;
  }
</style>
