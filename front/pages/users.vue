<template>
  <v-container>
    <v-row align="center" justify="center">
      <v-col cols="12">
        <v-text-field
        label="Username"
        v-model="name"
        prepend-icon=""
        type="text"
        />
        <v-btn color="primary" @click="createUser">ADD USER</v-btn>
      </v-col>
      <v-col cols="12">
          <h1>Hello, Qiita! </h1>
        </v-col>
      </v-row>

      <v-card
        class="mx-auto"
        max-width="300"
        tile
      >
          <v-list rounded>
            <v-subheader>USERS</v-subheader>
            <v-list-item-group color="primary">
              <v-list-item
                v-for="user in users"
                :key="user.id"
                @click="deleteUser(user.id)"
              >
                <v-list-item-content>
                  <v-list-item-title v-text=user.name />
                </v-list-item-content>
              </v-list-item>
            </v-list-item-group>
          </v-list>
      </v-card>
    </v-container>
</template>

<script>
import axios from "~/plugins/axios"

export default {
  data() {
    return {
      name: "",
      users: []
    }
  },
  created() {
    // ユーザーをaxiosで取得
    axios.get("/users").then(res => {
      if (res.data) {
          this.users = res.data
          }
        })
  },
  methods: {
    // ユーザーをaxiosで登録
    createUser(){
      axios.post("/users", {name: this.name})
    .then(res => {
      if (res.data) {
          this.users.push(res.data)
          this.name = "";
          alert("追加しました");
          }
        })
      },
      showId(id){
        alert(id);
      },
      deleteUser(id){
        axios.delete(`/users/${id}`)
        .then(res =>{
          if(res.data){
            this.users = {...this.user,...res.data};
            alert("削除しました");
          }
        })
      },
      updateUser(id){
        axios.put(`/users/${id}`,{name: 'hoge'})
        .then(res =>{
          if(res.data){
            this.users = {...this.user,...res.data};
            alert("更新しました");
          }
        })
      },
      showUser(id){
        axios.get(`/users/${id}`)
        .then(res =>{
          if(res.data){
            var {id,name} = res.data;
            alert('name: '+ name+ "\nid: " + id);
          }
        })
      }
  }
}
</script>