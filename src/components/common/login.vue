<template>
  <section class="login-wrap">
    <!-- <Col span="6"> -->
      <Form ref='form' class="left-form">
        <h3>轻点食</h3>
        <div class="fields">
          <Form-item style="margin-bottom: 0">
            <div style="display:flex;justify-content:center;">
              <img src="../assets/icon-logo.png" width="200" height="200">
            </div>
            
          </Form-item>
          
          <Form-item style="margin-bottom: 0">
              <h3>菜品排行</h3>
              <ul>
                <li style="font-size: 20px; margin-bottom: 10px; text-align: center;" v-for="menu in bestMenulist"><img src="../assets/fire.png" width="16" height="16" style="margin-right: 10px;">{{menu}}</li>
              </ul>
          </Form-item>

          <Form-item style="margin-bottom: 0">
              <h3>扫码就餐</h3>
              <div style="display:flex;justify-content:center;justify-content:center;">
                <img src="../assets/icon-code.png" width="200" height="200" style="border: 1px solid #ccc;border-radius: 12px;box-shadow: 2px 2px 32px 1px rgba(0, 0, 0, .45);">
              </div>
              
          </Form-item>
        </div>
      </Form>
    <!-- </Col> -->
    <Form ref='form' class="menu-form">
      <!-- <h3 @click="freshMenu()">今日菜谱</h3> -->
      <div class="css-menu-3d-btn" @click="freshMenu()">今日菜谱</div>
      <div class="fields">
        <Form-item style="margin-bottom: 0">
          <table style="width:100%; border-collapse:separate; border-spacing:20px;">
            <tr>
              <td style="vertical-align: middle; font-size: 20px;">
                午餐
              </td>
              <td v-for="menu in lunchMenuList" style="text-align: center;">
                <img :src="menu.ImageLocation" width="100px" height="100px">
                <p>{{menu.MenuName}}</p>
              </td>
            </tr>
          </table>
        </Form-item>
        
        <Form-item style="margin-bottom: 0">
          <table style="width:100%; border-collapse:separate; border-spacing:20px;">
            <tr>
              <td style="vertical-align: middle;font-size: 20px;">
                晚餐
              </td>
              <td v-for="menu in dinnerMenuList" style="text-align: center;">
                <img :src="menu.ImageLocation" width="100px" height="100px">
                <p>{{menu.MenuName}}</p>
              </td>
            </tr>
          </table>
        </Form-item>
      </div>
    </Form>
    <Form ref='form' class="login-form">
      <div style="display:flex;justify-content:center;">
        <div :class="[selectTabIndex == 1 && 'select-tab', 'tab']" @click="selectTab(1)"><span class="tab">员工登录</span></div>
        <div :class="[selectTabIndex == 2 && 'select-tab', 'tab']"@click="selectTab(2)"><span class="tab">访客登录</span></div>
      </div>
      <div class="fields">
        <Form-item prop="account" style="margin-bottom: 0">
          <Input
            size="large"
            class="field"
            v-model="login.account"
            placeholder="请填写账号"
            readonly
            width="520px"
            height="300px"
          >
          </Input>
        </Form-item>
        
        <Form-item prop="password" style="margin-bottom: 0">
          <Input
            size="large"
            class="field"
            v-model="login.password"
            type="password" 
            placeholder="请填写密码" 
            width="320px"
            readonly
            @keyup.native.enter="check"
          >
          </Input>
        </Form-item>
        
        <Form-item prop="num" v-if="selectTabIndex==2" style="margin-bottom: 0">
          <Input
            size="large"
            class="field"
            v-model="login.num"
            placeholder="访客请填写人数，不填默认为1" 
            width="320px"
            readonly  
            @keyup.native.enter="check"
          >
          </Input>
        </Form-item>
      </div>
      <div class="submit">
        <Button
          long
          @click.native="check"
          type="primary"
          :loading="loading"
          size="large"
        >
          {{ loading ? '登录中' : '登录' }}
        </Button>
      </div>
    </Form>
    <Form ref='form' class="number-form">
      <div class="fields">
        <table>
          <tr>
              <td @click="chooseNumber('1')">1</td>
              <td @click="chooseNumber('2')">2</td>
              <td @click="chooseNumber('3')">3</td>
          </tr>
          <tr>
              <td @click="chooseNumber('4')">4</td>
              <td @click="chooseNumber('5')">5</td>
              <td @click="chooseNumber('6')">6</td>
          </tr>
          <tr>
              <td @click="chooseNumber('7')">7</td>
              <td @click="chooseNumber('8')">8</td>
              <td @click="chooseNumber('9')">9</td>
          </tr>
          <tr>
              <td style="position: absolute; width: 57%; line-height: 60px;" @click="chooseNumber('0')">0</td>
              <td style="position: absolute; width: 28%;right: 6%;line-height: 60px;"@click="chooseNumber('C')">C</td>
          </tr>  
      </table>  
      </div>
    </Form>
    <Form ref='form' class="right-form">
        <div class="fields">
          <Form-item style="margin-bottom: 0">
            <h3>省四台职工就餐情况</h3>
            <ul style="margin-top: 111px;height:500px;width:110px;float:left;margin-left: 12px;" v-for="employeeList in allEmployeeList">
                <li class="employee-li" v-for="employee in employeeList">
                  
                  <img v-if="employee.Status == 1" src="../assets/icon-come.png" width="28" height="28">
                  <span v-if="employee.Status == 1" style="color: #f3cf78;">{{employee.NickName}}</span>

                  <img v-if="employee.Status == 0" src="../assets/icon-uncome.png" width="28" height="28">
                  <span v-if="employee.Status == 0" style="color: #515151;">{{employee.NickName}}</span>

                  <img v-if="employee.Status == 4" src="../assets/icon-come-noorder.png" width="28" height="28">
                  <span v-if="employee.Status == 4" style="color: #515151;">{{employee.NickName}}</span>

                  <img v-if="employee.Status == -1" src="../assets/icon-unselect.png" width="28" height="28">
                  <span v-if="employee.Status == -1" style="color: #aaa;">{{employee.NickName}}</span>

                  <div v-if="employee.time == 1 || employee.time == 2" style="font-size: 12px; color:#d35c5b; display:inline-block;border:1px solid #d35c5b;border-radius:10px;width:20px;height:20px;podition: relative;display:flex;justify-content:center;align-items:center;"><span style="">中</span></div>
                  <div v-if="employee.time == 2 || employee.time == 3" style="font-size: 12px; color:#353535; display:inline-block;border:1px solid #353535;border-radius:10px;width:20px;height:20px;podition: relative;display:flex;justify-content:center;align-items:center;"><span style="">晚</span></div>
                </li>
            </ul>
          </Form-item>
          <Form-item style="margin-bottom: 0;position: absolute;top: 60px;width:100%;">
              <table style="width: 100%;margin: 0 auto;">
                  <tr>
                    <td>
                      <div style="width:180px;height:28px;display:block;position:relative;left: 15px;"><img src="../assets/icon-wu.png" width="28" height="28" style="position: absolute;"><span style="font-size:14px;display:inline;position:absolute;left:28px;">今日午餐订餐人数：{{num1}}</span></div>
                    </td>
                    <td>
                      <div style="width:180px;height:28px;display:block;position:relative; right: 10px;"><img src="../assets/icon-wan.png" width="24" height="24" style="position: absolute;top: 3px;"><span style="font-size:14px;display:inline;position:absolute;left:28px;">今日晚餐订餐人数：{{num2}}</span></div>
                    </td>
                  </tr>
                  <tr>
                    <td>
                      <div style="width:180px;height:28px;display:block;position:relative;left: 15px;"><img src="../assets/icon-wu.png" width="28" height="28" style="position: absolute;"><span style="font-size:14px;display:inline;position:absolute;left:28px;">明日午餐订餐人数：{{num3}}</span></div>
                    </td>
                    <td>
                      <div style="width:180px;height:28px;display:block;position:relative; right: 10px;"><img src="../assets/icon-wan.png" width="24" height="24" style="position: absolute;top: 3px;"><span style="font-size:14px;display:inline;position:absolute;left:28px;">明日晚餐订餐人数：{{num4}}</span></div>
                    </td>
                  </tr>
              </table>
              <div style="width:130px;height:28px;display:inline-block;position:relative;"><img src="../assets/icon-come.png" width="28" height="28"><span style="font-size:14px;display:inline;position:absolute;">订餐并就餐：{{state3}}</span></div>
              <div style="width:130px;height:28px;display:inline-block;position:relative;"><img src="../assets/icon-uncome.png" width="25" height="25" style="position: absolute;top:1px;"><span style="font-size:14px;display:inline;position:absolute;left:28px;">订餐未就餐：{{state2}}</span></div>
              <div style="width:130px;height:28px;display:inline-block;position:relative;"><img src="../assets/icon-come-noorder.png" width="25" height="25" style="position: absolute;top:1px;"><span style="font-size:14px;display:inline;position:absolute;left:28px;">就餐未订餐：{{state4}}</span></div>
              <div style="width:130px;height:28px;display:inline-block;position:relative"><img src="../assets/icon-unselect.png" width="28" height="28" style="position: absolute;top:-11px;"><span style="font-size:14px;display:inline;position:absolute;left:28px;top:-11px">未订餐：{{state1}}</span></div>
              
          </Form-item>
          <div style="position: absolute;bottom: 35px;left: 0;right: 0;">
            <div @click="sendMsg" class="css-3d-btn" style="" >吃饭啦!</div>
          </div>
        </div>
      </Form>
    <canvas id="J_loginBackground"></canvas>
  </section>
</template>

<script>
  import { render } from '../config/canvas'
  import axios from 'axios'
  import qs from 'qs'
  export default {
    data () {
      return {
        ip:'47.96.184.230',
        today: '',
        login: {
          account: '',
          password: '',
          num: ''
        },
        state1: 0,
        state2: 0,
        state3: 0,
        state4: 0,
        num1: 0,//今日午餐订餐人数
        num2: 0,//今日晚餐订餐人数
        num3: 0,//明日午餐订餐人数
        num4: 0,//明日晚餐订餐人数
        remember: [],
        loading: false,
        bestMenulist: [],
        lunchMenuList: [],
        dinnerMenuList: [],
        employeeState: [],
        employeeState2:[],
        employeeList: [],
        allEmployeeList: [],
        selectTabIndex: 1,
        focusInput: '',
        focusItemId: 'account'
      }
    },
    methods: {
      render: render,
      check () {
        let vm = this
        this.login.account = document.getElementById('account').value
        this.login.password = document.getElementById('password').value
        if (this.selectTabIndex === 2) {
          this.login.num = document.getElementById('number').value
        }
        if (this.login.account === '') {
          vm.addDom()
          vm.focusItemId = 'account'
          let dom = document.getElementById(vm.focusItemId)
          dom.focus()
        } else if (this.login.password === '') {
          vm.addDom()
          vm.focusItemId = 'password'
          let dom = document.getElementById(vm.focusItemId)
          dom.focus()
        }
        else {
          let EatingTime = 0
          let now = new Date().getHours()
          if (now >= 10 && now <= 14) {
            EatingTime = 2
          }
          if (now >= 17 && now <= 20) {
            EatingTime = 3
          }
          if (EatingTime === 0) {
            vm.$Notice.error({
              title: '当前时间无法登录',
              duration: 3
            })
            setTimeout(()=>{
              document.getElementsByClassName('ivu-input')[0].setAttribute('id','account')
              document.getElementById('account').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false) 
              document.getElementsByClassName('ivu-input')[1].setAttribute('id','password')
              document.getElementById('password').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false)
              if (vm.selectTabIndex === 2) {
                document.getElementsByClassName('ivu-input')[2].setAttribute('id','number')
                document.getElementById('number').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false)
              }
              document.getElementById('account').value = ""
              document.getElementById('password').value = ""
              if (vm.selectTabIndex === 2) {
                document.getElementById('number').value = ""
              }
              vm.focusItemId = 'account'
            },500)
          } else {
            this.loading = true
            axios.post('http://'+ this.ip +'/canteen/loginForSpecial.php', qs.stringify({
              'EmployeeName': this.login.account,
              'Password': this.login.password,
              'Num': this.selectTabIndex === 1 ? 0 : (this.login.num === '' ? 1 : this.login.num),
              'EatingDate': this.today,
              'EatingTime': EatingTime
            })).then(function (res) {
              let data = res.data
              if (data.return_code === 1) {
                vm.loading = false
                vm.$Notice.success({
                  title: '就餐成功',
                  duration: 3
                })
                vm.queryMealStatus()
                vm.login.account = ''
                vm.login.password = ''
                vm.login.num = ''
                document.getElementsByClassName('ivu-input')[0].setAttribute('id','account')
                document.getElementById('account').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false) 
                document.getElementsByClassName('ivu-input')[1].setAttribute('id','password')
                document.getElementById('password').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false)
                if (vm.selectTabIndex === 2) {
                  document.getElementsByClassName('ivu-input')[2].setAttribute('id','number')
                  document.getElementById('number').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false)
                }
                document.getElementById('account').value = ""
                document.getElementById('password').value = ""
                if (vm.selectTabIndex === 2) {
                  document.getElementById('number').value = ""
                }
              } else if (data.return_code === 0) {
                vm.loading = false
                if (data.return_msg === 'wrong') {
                  vm.$Notice.error({
                    title: '账号密码有误，请重新填写',
                    duration: 3
                  })
                  document.getElementsByClassName('ivu-input')[0].setAttribute('id','account')
                  document.getElementById('account').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false) 
                  document.getElementsByClassName('ivu-input')[1].setAttribute('id','password')
                  document.getElementById('password').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false)
                  if (vm.selectTabIndex === 2) {
                    document.getElementsByClassName('ivu-input')[2].setAttribute('id','number')
                    document.getElementById('number').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false)
                  }
                  document.getElementById('account').value = ""
                  document.getElementById('password').value = ""
                  if (vm.selectTabIndex === 2) {
                    document.getElementById('number').value = ""
                  }
                }
                if (data.return_msg === 'Already has order') {
                  vm.$Notice.success({
                    title: '无需重复订餐哦',
                    duration: 3
                  })
                  document.getElementsByClassName('ivu-input')[0].setAttribute('id','account')
                  document.getElementById('account').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false) 
                  document.getElementsByClassName('ivu-input')[1].setAttribute('id','password')
                  document.getElementById('password').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false)
                  if (vm.selectTabIndex === 2) {
                    document.getElementsByClassName('ivu-input')[2].setAttribute('id','number')
                    document.getElementById('number').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false)
                  }
                  document.getElementById('account').value = ""
                  document.getElementById('password').value = ""
                  if (vm.selectTabIndex === 2) {
                    document.getElementById('number').value = ""
                  }
                }
              }
            }).catch(function (error) {
              console.log(error)
            })
          }
        }
      },
      freshMenu () {
        // let vm = this
        // let day = JSON.stringify(new Date().getDate()).length > 1 ? new Date().getDate() : '0' + new Date().getDate()
        // let month = JSON.stringify(new Date().getDate()).length > 1 ? new Date().getMonth() + 1 : '0' + (new Date().getMonth() + 1)
        // vm.today = new Date().getFullYear() + '-' + month + '-' + day
        // axios.post('http://' + this.ip + '/canteen/getOneDayMenu.php', qs.stringify({
        //   EatingDate: vm.today
        // })).then(function (res) {
        //   let data = res.data
        //   if (data.return_code === 1) {
        //     console.log(data.return_msg)
        //     vm.lunchMenuList = data.return_msg.lunch
        //     vm.dinnerMenuList = data.return_msg.dinner
        //   }
        // }).catch(function (error) {
        //   console.log(error)
        // })
        location.reload()
      },
      selectTab (index) {
        let vm = this
        this.selectTabIndex = index
        vm.focusItemId = 'account'
        vm.$nextTick(()=>{
          if (document.getElementsByClassName('ivu-input').length > 2 && document.getElementById('number') == null) {
            document.getElementsByClassName('ivu-input')[2].setAttribute('id','number')
            document.getElementById('number').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false)
          }
        })
      },
      chooseNumber (num) {
        let vm = this
        // console.log(vm.focusItemId)
        let dom = document.getElementById(vm.focusItemId)
        dom.focus()
        if (num == 'C') {
          console.log('111')
          dom.value = dom.value.substring(0,dom.value.length-1)
        } else {
          dom.value = dom.value + num
        }
      },
      queryMealStatus () {
        let vm = this
        axios.post('http://'+ this.ip +'/canteen/queryMealStatus.php', qs.stringify({
          EatingDate: vm.today
        })).then(function (res) {
          let data = res.data
          if (data.return_code === 1) {
            vm.allEmployeeList = []
            let allEmployee = data.return_msg.employee
            // let a = []
            // for (var i of allEmployee) {
            //   a.push(i)
            //   a.push(i)
            //   a.push(i)
            // }
            if (allEmployee.length > 20) {
              vm.allEmployeeList.push(allEmployee.slice(0, 10))
              vm.allEmployeeList.push(allEmployee.slice(10, 20))
              vm.allEmployeeList.push(allEmployee.slice(20))
            } else {
              if (allEmployee.length > 10) {
                vm.allEmployeeList.push(allEmployee.slice(0,10))
                vm.allEmployeeList.push(allEmployee.slice(10))
              } else {
                vm.allEmployeeList.push(allEmployee)
              }
            }
            console.log(vm.allEmployeeList)
            let countArr = data.return_msg.count
            vm.state1 = countArr[0] //未订餐 state=-1
            vm.state2 = countArr[1] //订餐还没来 state=0
            vm.state3 = countArr[2] //订餐且就餐 state=1
            vm.state4 = countArr[3] //就餐未订餐 state=4
            let countPeopleArr = data.return_msg.countPeople
            let countTomorrow = data.return_msg.countTomorrow
            vm.num1 = countPeopleArr[0] //今日午餐订餐人数
            vm.num2 = countPeopleArr[1] //今日晚餐订餐人数
            vm.num3 = countTomorrow[0] //明日午餐订餐人数
            vm.num4 = countTomorrow[1] //明日晚餐订餐人数
          }
        }).catch(function (error) {
          console.log(error)
        })
      },
      addDom () {
        let vm = this
        document.getElementsByClassName('ivu-input')[0].setAttribute('id','account')
        document.getElementById('account').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false) 
        document.getElementsByClassName('ivu-input')[1].setAttribute('id','password')
        document.getElementById('password').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false)
        if (vm.selectTabIndex === 2) {
          document.getElementsByClassName('ivu-input')[2].setAttribute('id','number')
          document.getElementById('number').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false)
        }
      },
      sendMsg () {
        let EatingTime = 0
        let now = new Date().getHours()
        let vm = this
        if (now >= 10 && now <= 14) {
          EatingTime = 2
        }
        if (now >= 16 && now <= 20) {
          EatingTime = 3
        }
        if (EatingTime == 2 || EatingTime == 3) {
          axios.post('http://' + this.ip + '/canteen/sendMessage.php', qs.stringify({
            EatingDate: this.today,
            EatingTime: EatingTime
          })).then(function (res) {
            let data = res.data
            if (data.return_code === 1) {
              vm.$Notice.success({
                title: '已通知员工来就餐',
                duration: 3
              })
            } else {
              vm.$Notice.error({
                title: '当前没有订餐员工',
                duration: 3
              })
            }
          }).catch(function (error) {
            console.log(error)
          })
        } else{
          vm.$Notice.error({
            title: '当前时间段不可通知员工就餐',
            duration: 3
          })
        }
      }
    },
    watch: {
      '$route' () {
        if (this.$route.name === 'login') {
          this.render()
        }
      },
      focusItemId (newVal, oldVal) {
        console.log('newVal',newVal)
      }
    },
    created () {
      let vm = this
      let day = JSON.stringify(new Date().getDate()).length > 1 ? new Date().getDate() : '0' + new Date().getDate()
      let month = JSON.stringify(new Date().getDate()).length > 1 ? new Date().getMonth() + 1 : '0' + (new Date().getMonth() + 1)
      vm.today = new Date().getFullYear() + '-' + month + '-' + day
      let EatingTime = 0
      let now = new Date().getHours()
      if (now >= 10 && now <= 14) {
        EatingTime = 2
      }
      if (now >= 16 && now <= 20) {
        EatingTime = 3
      }
      setInterval(()=>{
        vm.queryMealStatus()
      },1000) 
      
      vm.queryMealStatus()

      axios.post('http://' + this.ip + '/canteen/getOneDayMenu.php', qs.stringify({
        EatingDate: vm.today
      })).then(function (res) {
        let data = res.data
        if (data.return_code === 1) {
          console.log(data.return_msg)
          vm.lunchMenuList = data.return_msg.lunch
          vm.dinnerMenuList = data.return_msg.dinner
        }
      }).catch(function (error) {
        console.log(error)
      })

      axios.get('http://' + this.ip + '/canteen/menuRank.php', {
      }).then(function (res) {
        let data = res.data
        if (data.return_code === 1) {
          vm.bestMenulist = data.return_msg.slice(0, 5)
          console.log('bestMenulist', vm.bestMenulist)
        }
      }).catch(function (error) {
        console.log(error)
      })

      vm.$nextTick(()=>{
        document.getElementsByClassName('ivu-input')[0].setAttribute('id','account')
        document.getElementById('account').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false) 
        document.getElementsByClassName('ivu-input')[1].setAttribute('id','password')
        document.getElementById('password').addEventListener('click',function(){vm.focusItemId = document.activeElement.id},false) 
      })
      
    },
    mounted () {
      this.render()
    }
  }
</script>

<style lang="less" scoped>
  .login-wrap {
    position: relative;
    z-index: 0;
    top: 0;
    left: 0;
    /*overflow: hidden;*/
    width: 100%;
    height: 100%;
    background: #111;
    canvas {
      width: 100%;
      height: 100%;
      position: fixed;
      background-color: #000;
    }
  }
  .left-form {
    position: absolute;
    z-index: 1;
    top: 100px;
    bottom: 0;
    left: 4%;
    right: 0;
    /*margin: 0 auto;*/
    padding: 16px 20px 0;
    width: 22%;
    height: 900px;
    font-size: 14px;
    background: #fff;
    border: 1px solid #ccc;
    border-radius: 12px;
    box-shadow: 2px 2px 32px 1px rgba(0, 0, 0, .45);
    opacity: .85;
    h3 {
      margin-top: 0;
      margin-bottom: 0;
      padding: 12px 0;
      font-weight: normal;
      font-size: 22px;
      text-align: center;
    }
    .field {
      display: block;
      margin: 0 auto;
      padding: 6px 0;
    }
  }
  .menu-form {
    position: absolute;
    z-index: 1;
    top: 100px;
    bottom: 0;
    left: 0;
    right: 0;
    margin: 0 auto;
    padding: 16px 20px 0;
    width: 43%;
    height: 500px;
    font-size: 14px;
    background: #fff;
    border: 1px solid #ccc;
    border-radius: 12px;
    box-shadow: 2px 2px 32px 1px rgba(0, 0, 0, .45);
    opacity: .85;
    .css-menu-3d-btn {
       cursor: pointer;
       position: relative;
       color: rgba(255, 255, 255, 1);
       text-decoration: none;
       background-color: rgba(219, 87, 51, 1);
       /*font-family: "微软雅黑";*/
       font-weight: 700;
       font-size: 2em;
       display: block;
       /*padding: 65px 10px;*/
       border-radius: 20px;
       /* let's use box shadows to make the button look more 3-dimensional */
       box-shadow: 0px 9px 0px rgba(219, 31, 5, 1), 0px 9px 25px rgba(0, 0, 0, .7);
       margin: 0 auto;
       /*margin-top: 50px;*/
       width: 290px;
       height: 60px;
       line-height: 60px;
       text-align: center;
       vertical-align: center;
       -webkit-transition: all .1s ease;
       -moz-transition: all .1s ease;
       transition: all .1s ease;
    }

/* now if we make the box shadows smaller when the button is clicked, it'll look like the button has been "pushed" */

      .css-menu-3d-btn:active{
       box-shadow: 0px 3px 0px rgba(219, 31, 5, 1), 0px 3px 6px rgba(0, 0, 0, .9);
       position: relative;
       top: -3px;
      }
    h3 {
      width: 20%;
      margin-top: 0;
      margin-bottom: 0;
      margin: 0 auto;
      padding: 12px 0;
      font-weight: normal;
      font-size: 22px;
      text-align: center;
      border-radius: 5px;
      border: 1px solid rgba(0, 0, 0, .45);
      cursor: pointer;
    }
    .field {
      display: block;
      margin: 0 auto;
      padding: 6px 0;
    }
  }
  .login-form {
    position: absolute;
    z-index: 5;
    top: 700px;
    bottom: 0;
    left: 28.5%;
    right: 0;
    /*margin: 0 auto;*/

    padding: 16px 20px 0;
    width: 23%;
    height: 300px;
    font-size: 14px;
    background: #fff;
    border: 1px solid #ccc;
    border-radius: 12px;
    box-shadow: 2px 2px 32px 1px rgba(0, 0, 0, .45);
    opacity: .85;
    h3 {
      margin-top: 0;
      margin-bottom: 0;
      padding: 12px 0;
      font-weight: normal;
      font-size: 22px;
      text-align: center;
    }
    .tab {
      display: inline-block;
      font-size: 18px;
      margin-bottom: 10px;
      width: 50%; 
      text-align: center; 
      cursor: pointer; 
      border-bottom: 3px solid #fff;
    }
    .select-tab {
      color: #efb336;
      border-bottom: 3px solid #efb336;
    }
    .field {
      display: block;
      margin: 0 auto;
      padding: 6px 0;
    }
    .submit {
      font-size: 20px;
      padding: 12px 0;
    }
  }
  .number-form {
    position: absolute;
    z-index: 5;
    top: 700px;
    bottom: 0;
    left: 53.5%;
    /*right: 30%;*/
    /*margin: 0 auto;*/
    padding: 16px 20px 0;
    width: 18%;
    height: 300px;
    font-size: 24px;
    background: #fff;
    border: 1px solid #ccc;
    border-radius: 12px;
    box-shadow: 2px 2px 32px 1px rgba(0, 0, 0, .45);
    opacity: .85;
    table{  
            margin-top: 5px;
            width: 100%;  
            height: 100%;  
        }  
        td{  
            width: 60px;
            height: 60px;
            border: 1px solid #ccc;
            border-radius: 10px;
            text-align: center;
            margin: 2px;
        }  
       td:hover{  
          cursor: pointer;
          color: #efb336;
       }  
  }
  table p {
    font-size: 20px;
  }
  .right-form {
    position: absolute;
    z-index: 1;
    top: 100px;
    bottom: 0;
    right: 3%;
    /*right: 0px;*/
    /*margin: 0 auto;*/
    padding: 16px 20px 0;
    width: 23%;
    height: 900px;
    font-size: 14px;
    background: #fff;
    border: 1px solid #ccc;
    border-radius: 12px;
    box-shadow: 2px 2px 32px 1px rgba(0, 0, 0, .45);
    opacity: .85;
    h3 {
      margin-top: 0;
      margin-bottom: 0;
      padding: 12px 0;
      font-weight: normal;
      font-size: 22px;
      text-align: center;
    }
    .employee-li {
        font-size: 20px; 
        margin-bottom: 15px;
        display:flex;
        align-items:center;
    }
    .field {
      display: block;
      margin: 0 auto;
      padding: 6px 0;
    }
    .css-3d-btn{
       cursor: pointer;
       position: relative;
       color: rgba(255, 255, 255, 1);
       text-decoration: none;
       background-color: rgba(219, 87, 51, 1);
       /*font-family: "微软雅黑";*/
       font-weight: 700;
       font-size: 3em;
       display: block;
       padding: 65px 10px;
       border-radius: 90px;
       /* let's use box shadows to make the button look more 3-dimensional */
       box-shadow: 0px 9px 0px rgba(219, 31, 5, 1), 0px 9px 25px rgba(0, 0, 0, .7);
       margin: 0 auto;
       /*margin-top: 50px;*/
       width: 180px;
       height: 180px;
       text-align: center;
       -webkit-transition: all .1s ease;
       -moz-transition: all .1s ease;
       transition: all .1s ease;
    }

/* now if we make the box shadows smaller when the button is clicked, it'll look like the button has been "pushed" */

      .css-3d-btn:active{
       box-shadow: 0px 3px 0px rgba(219, 31, 5, 1), 0px 3px 6px rgba(0, 0, 0, .9);
       position: relative;
       top: -6px;
      }

  }
</style>
