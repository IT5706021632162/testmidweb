<template>
  <div class="hello">

    <a class="button is-info is-outlined " @click="sign()">signin</a>
    <a class="button is-info is-outlined " @click="signOut()">signout</a>
    <br>
    <img :src="photoURL"> <br>
    {{ displayName }}
    <router-view></router-view><br><br>
    <div class="columns">
            <div class="column"></div>
                <div class="column is-two-thirds">
                  <div class="columns">
                    <div class="column">
                      <div class="field">
                        <label class="label">รหัสนักศึกษา</label>
                        <div class="control">
                          <input class="input is-info" type="number"  v-model="code" placeholder="รหัสนักศึกษา">
                        </div>
                      </div>
                    </div>

                    <div class="column">
                      <div class="field">
                        <label class="label">ชื่อ</label>
                        <div class="control">
                          <input class="input is-info" type="text" v-model="name" placeholder="ชื่อ">
                        </div>
                      </div>
                    </div>

                    <div class="column">
                      <div class="field">
                        <label class="label">นามสกุล</label>
                          <div class="control">
                            <input class="input is-info" type="text" v-model="surname" placeholder="นามสกุล">
                          </div>
                        </div>
                    </div>

                    <div class="column">
                      <div class="field">
                        <label class="label">เบอร์โทรศัพท์</label>
                          <div class="control">
                            <input class="input is-info" type="number" v-model="telephone" placeholder="เบอร์โทรศัพท์">
                          </div>
                        </div>
                    </div>

                    <div class="column">
                      <div class="field">
                        <label class="label">เพศ</label>
                        <div class="control">
                            <input type="radio" id="one" value="ชาย" v-model="sex">
                            <label for="one">ชาย</label>

                            <input type="radio" id="two" value="หญิง" v-model="sex">
                            <label for="two">หญิง</label>

                        </div>
                     </div>
                   </div>

                    <div class="column">
                      <label class="label">อายุ</label>
                      <div class="control">
                      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  <div class="select">
                          <select v-model="selectedAge">
                            <option>20</option>
                            <option>21</option>
                            <option>22</option>
                            <option>23</option>
                            <option>24</option>
                          </select>
                        </div>
                      </div>
                    </div>

                      <!-- <div class="column">
                      <a class="button is-info is-focused" @click="insert()">Add</a>
                    </div> -->

                </div>
              </div>
            <div class="column"></div>
          </div>

            <div class="column">
              <div class="column"></div>
                  <a class="button is-info is-focused" @click="insert()">Add</a>
              <div class="column"></div>
            </div>

          <br>

          <div class="columns">
            <div class="column"></div>
              <div class="column is-11 is-offset-1">
                <div  class="tablestudent" >
                  <table class="table" >
                    <thead>
                      <tr>
                        <th scope="col">#</th>
                        <th scope="col">Code</th>
                        <th scope="col">Name</th>
                        <th scope="col">Surname</th>
                        <th scope="col">Telephone</th>
                        <th scope="col">sex</th>
                        <th scope="col">Age</th>
                        <th scope="col">Update</th>
                        <th scope="col">Delete</th>
                      </tr>
                    </thead>
                    <tbody   v-if="checkEdit !== key" v-for = "(show, key, count) in showstudent"  :key ="show.num">
                      <tr>
                        <td>  {{count+1}}  </td>
                        <td>  {{show.code}}  </td>
                        <td>  {{show.name}} </td>
                        <td>  {{show.surname}} </td>
                        <td>  {{show.telephone}} </td>
                        <td>  {{show.sex}} </td>
                        <td>  {{show.selectedAge}} </td>
                        <td>      <a class="button is-info is-outlined"  @click="swap(key)">Update</a> </td>
                        <td>      <a class="button is-danger is-outlined" @click="Delete(key)">Delete</a> </td>
                      </tr>
                    </tbody>
                    <tbody v-else >
                      <tr >
                         <td>  {{count+1}}  </td>
                          <td><input class="input is-info" type="number"  v-model="show.code" placeholder="รหัสนักศึกษา"> </td>
                        <td>  <input class="input is-info" type="text" v-model="show.name" placeholder="ชื่อ"> </td>
                        <td>  <input class="input is-info" type="text" v-model="show.surname" placeholder="นามสกุล"> </td>
                        <td>  <input class="input is-info" type="number" v-model="show.telephone" placeholder="เบอร์โทรศัพท์"> </td>
                        <td>
                          <div class="column">
                            <div class="field">
                            <div class="control">

                                <input type="radio" id="one" value="ชาย" v-model="show.sex">
                                <label for="one">ชาย</label>

                                <input type="radio" id="two" value="หญิง" v-model="show.sex">
                                <label for="two">หญิง</label>

                              </div>
                            </div>
                          </div>
                        </td>
                        <td>
                            <div class="column">
                            <div class="control">
                              <div class="select">
                                <select v-model="show.selectedAge">
                                  <option>20</option>
                                  <option>21</option>
                                  <option>22</option>
                                  <option>23</option>
                                  <option>24</option>
                                </select>
                              </div>
                            </div>
                          </div>
                      </td>

                        <td> <button type="button" class="button button is-success " name="buttonAdd"  @click="Update(key,show.code,show.name,show.surname,show.telephone,show.sex,show.selectedAge)" >Save</button> </td>
                        <td> <button type="button" class="button is-danger" name="buttonAdd" @click="cancel()" >Cancel</button> </td>

                      </tr>
                    </tbody>
                  </table>
              </div>
            </div>
          </div>
  </div>
</template>

<script>
import firebase from 'firebase'
// Initialize Firebase
var config = {
  apiKey: 'AIzaSyCjl88WLHmFMuCgyfsTdHiibOLVJV9FIrs',
  authDomain: 'testmud-36590.firebaseapp.com',
  databaseURL: 'https://testmud-36590.firebaseio.com',
  projectId: 'testmud-36590',
  storageBucket: 'testmud-36590.appspot.com',
  messagingSenderId: '1097521811911'
}
firebase.initializeApp(config)
var provider = new firebase.auth.FacebookAuthProvider()
provider.addScope('public_profile')
provider.setCustomParameters({
  'display': 'popup'
})
export default {
  name: 'HelloWorld',
  data () {
    return {
      showstudent: '',
      cout: 0,
      checkEdit: '',
      displayName: '',
      photoURL: '',
      data: {
        code: '',
        name: '',
        surname: '',
        telephone: '',
        sex: '',
        selectedAge: ''
      }
    }
  },
  created: function () {
    this.pullData()
  },
  methods: {
    pullData: function () {
      let that = this
      firebase.database().ref('/student/').once('value').then(function (snapshot) {
        that.showstudent = snapshot.val()
      })
    },
    insert () {
      this.data.code = this.code
      this.data.name = this.name
      this.data.surname = this.surname
      this.data.telephone = this.telephone
      this.data.sex = this.sex
      this.data.selectedAge = this.selectedAge
      firebase.database().ref('student').push(this.data)
      this.code = ''
      this.name = ''
      this.surname = ''
      this.telephone = ''
      this.sex = ''
      this.selectedAge = ''
      this.pullData()
    },
    Update (key, code, name, sur, tel, sex, selectedAge) {
      firebase.database().ref('/student/').child(key).update({
        code: code,
        name: name,
        surname: sur,
        telephone: tel,
        sex: sex,
        selectedAge: selectedAge
      })
      this.pullData()
      this.checkEdit = ''
    },
    Delete (key) {
      firebase.database().ref('student').child(key).remove()
      this.pullData()
    },
    swap: function (key) {
      this.checkEdit = key
    },
    cancel () {
      this.checkEdit = ''
    },
    sign () {
      console.log('yes')
      var vm = this
      firebase.auth().signInWithPopup(provider).then(function (result) {
        var token = result.credential.accessToken
        var user = result.user
        console.log(token, user)
        console.log('displayName :: ', user.displayName)
        console.log('photoURL ::', user.photoURL)
        vm.displayName = user.displayName
        vm.photoURL = user.photoURL
      }).catch(function (error) {
        console.log(error)
      })
    },
    signOut () {
      var vm = this
      firebase.auth().signOut().then(function () {
        console.log('logOut')
        vm.displayName = ''
        vm.photoURL = ''
      }, function (error) {
        console.log(error)
      })
    }
  }
}

</script>
