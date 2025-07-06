<template>
  <div class="setting-main">
    <div class="flex-container">
      <div class="left">
        <p class="section-title">{{$t('m.ChangePassword')}}</p>
        <Form class="setting-content" ref="formPassword" :model="formPassword" :rules="rulePassword">
          <FormItem label="현재 비밀번호" prop="old_password">
            <Input v-model="formPassword.old_password" type="password"/>
          </FormItem>
          <FormItem label="새 비밀번호" prop="new_password">
            <Input v-model="formPassword.new_password" type="password"/>
          </FormItem>
          <FormItem label="비밀번호 확인" prop="again_password">
            <Input v-model="formPassword.again_password" type="password"/>
          </FormItem>
          <FormItem v-if="visible.tfaRequired" label="이중 인증 코드" prop="tfa_code">
            <Input v-model="formPassword.tfa_code"/>
          </FormItem>
          <FormItem v-if="visible.passwordAlert">
            <Alert type="success">5초 이후 다시 로그인 해 주세요.</Alert>
          </FormItem>
          <Button type="primary" @click="changePassword">{{$t('m.Update_Password')}}</Button>
        </Form>
      </div>

      <div class="middle separator"></div>

      <div class="right">
        <p class="section-title">{{$t('m.ChangeEmail')}}</p>
        <Form class="setting-content" ref="formEmail" :model="formEmail" :rules="ruleEmail">
          <FormItem label="현재 비밀번호" prop="password">
            <Input v-model="formEmail.password" type="password"/>
          </FormItem>
          <FormItem label="현재 이메일">
            <Input v-model="formEmail.old_email" disabled/>
          </FormItem>
          <FormItem label="새 이메일" prop="new_email">
            <Input v-model="formEmail.new_email"/>
          </FormItem>
          <FormItem v-if="visible.tfaRequired" label="이중 인증 코드" prop="tfa_code">
            <Input v-model="formEmail.tfa_code"/>
          </FormItem>
          <Button type="primary" @click="changeEmail">{{$t('m.ChangeEmail')}}</Button>
        </Form>
      </div>
    </div>
  </div>
</template>

<script>
  import api from '@oj/api'
  import { FormMixin } from '@oj/components/mixins'

  export default {
    mixins: [FormMixin],
    data () {
      const oldPasswordCheck = [{required: true, trigger: 'blur', min: 6, max: 20, message: '현재 비밀번호를 입력해 주세요.'}]
      const tfaCheck = [{required: true, trigger: 'change', message: '이중 인증 코드를 입력해 주세요.'}]
      const CheckAgainPassword = (rule, value, callback) => {
        if (value !== this.formPassword.new_password) {
          callback(new Error('비밀번호가 일치하지 않아요.'))
        }
        callback()
      }
      const CheckNewPassword = (rule, value, callback) => {
        if (this.formPassword.old_password !== '') {
          if (this.formPassword.old_password === this.formPassword.new_password) {
            callback(new Error('이전 비밀번호와 다른 비밀번호를 입력해 주세요.'))
          } else {
            // 对第二个密码框再次验证
            this.$refs.formPassword.validateField('again_password')
          }
        }
        callback()
      }
      return {
        loading: {
          btnPassword: false,
          btnEmail: false
        },
        visible: {
          passwordAlert: false,
          emailAlert: false,
          tfaRequired: false
        },
        formPassword: {
          tfa_code: '',
          old_password: '',
          new_password: '',
          again_password: ''
        },
        formEmail: {
          tfa_code: '',
          password: '',
          old_email: '',
          new_email: ''
        },
        rulePassword: {
          old_password: oldPasswordCheck,
          new_password: [
            {required: true, trigger: 'blur', message: '새 비밀번호를 입력해 주세요.'},
            {min: 6, max: 20, trigger: 'blur', message: '비밀번호는 6자 이상 20자 이하로 입력해 주세요.'},
            {validator: CheckNewPassword, trigger: 'blur'}
          ],
          again_password: [
            {required: true, validator: CheckAgainPassword, trigger: 'change'}
          ],
          tfa_code: tfaCheck
        },
        ruleEmail: {
          password: oldPasswordCheck,
          new_email: [
            {required: true, trigger: 'change', message: '새 이메일을 입력해 주세요.'},
            {type: 'email', trigger: 'change', message: '이메일 형식이 올바르지 않아요.'}
          ],
          tfa_code: tfaCheck
        }
      }
    },
    mounted () {
      this.formEmail.old_email = this.$store.getters.user.email || ''
    },
    methods: {
      changePassword () {
        this.validateForm('formPassword').then(valid => {
          this.loading.btnPassword = true
          let data = Object.assign({}, this.formPassword)
          delete data.again_password
          if (!this.visible.tfaRequired) {
            delete data.tfa_code
          }
          api.changePassword(data).then(res => {
            this.loading.btnPassword = false
            this.visible.passwordAlert = true
            this.$success('비밀번호를 변경했어요.')
            setTimeout(() => {
              this.visible.passwordAlert = false
              this.$router.push({name: 'logout'})
            }, 5000)
          }, res => {
            if (res.data.data === 'tfa_required') {
              this.visible.tfaRequired = true
            }
            this.loading.btnPassword = false
          })
        })
      },
      changeEmail () {
        this.validateForm('formEmail').then(valid => {
          this.loading.btnEmail = true
          let data = Object.assign({}, this.formEmail)
          if (!this.visible.tfaRequired) {
            delete data.tfa_code
          }
          api.changeEmail(data).then(res => {
            this.loading.btnEmail = false
            this.visible.emailAlert = true
            this.$success('이메일을 변경했어요.')
            this.$refs.formEmail.resetFields()
          }, res => {
            if (res.data.data === 'tfa_required') {
              this.visible.tfaRequired = true
            }
          })
        })
      }
    }
  }
</script>

<style lang="less" scoped>

  .flex-container {
    justify-content: flex-start;
    .left {
      flex: 1 0;
      width: 250px;
      padding-right: 5%;
    }
    > .middle {
      flex: none;
    }
    .right {
      flex: 1 0;
      width: 250px;
    }
  }
</style>

