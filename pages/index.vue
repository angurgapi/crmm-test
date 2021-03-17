<template>
  <div class="wrapper">
    <div class='users' ref='users'>
      <div class='header'>
          <h2>Рейтинг пользователей</h2>
          <button @click="sortUsers"><img class='sort' src="/img/sort.jpg"></button>
        </div>

       <div class='user' v-for='user in sortedUsers' @click="userModal(`${user.id}`)">
         <div class='user-left'>
           <img class='avatar' :src="`${user.avatar}`">
           <div class='user-name'>
            <p>Пользователь</p>
             <p>{{ user.name }}</p>
           </div>
         </div>
         <div class="user-right">
          <img class='rating' src="/img/star.png">
           <p>{{ user.rating}}</p>
         </div>
       </div>

    </div>
    <div class="dimmer" ref='dimmer'></div>

    <div class='user-modal' ref='modal'>
      <div class="modal-card">
        <div class="modal-header">
          <h2>Профиль пользователя</h2>
          <p class="close-modal">x</p>
        </div>
        
        <div class="modal-body">
          <div class="modal-left">
            <div class="user-avatar"></div>
          </div>
          <div class="modal-right">
            <p>Имя:</p>       
            <p>Баллы:</p>
            <p>Позиция в рейтинге</p>    
          </div>
        </div>
        <div class="modal-footer">
          <p>О себе:</p>
          <p class='description' ref='description'></p>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  head: {
    title: "Users list"
  },
  data(){
    return{      
      users: [], 
      sortDirection: 'desc'
    }
  },
  async asyncData(context) {
    try {
      return await fetch("https://my-json-server.typicode.com/Vespand/crmm-tasks/users")
        .then(res => res.json())
        .then(data => {
          data.sort((a,b)=>{
            return b.rating - a.rating
          })
          return { users: data }
        });
    } catch (e) {
      console.error("SOMETHING WENT WRONG :" + e);
    }
  },

  computed: {
    sortedUsers() {
      return this.users.sort((a, b) => {       
        if(this.sortDirection === 'desc'){
          return b.rating - a.rating
           }
        return a.rating - b.rating 
          });
        }
    
},
  methods:{
    sortUsers(){
      this.sortDirection = this.sortDirection === 'asc' ? 'desc' : 'asc'
    },
    userModal(id){
      console.log(id)
      let currentUser = this.users.find(user => {
        return user.id === id
        })
      this.$refs.dimmer.style.display = 'flex'
      this.$refs.modal.style.display = 'flex'
      console.log(currentUser.name)

    }
  }
};
</script>
<style lang="sass">
.users
  display: flex
  flex-direction: column
  justify-content: center
  align-items: center
  width: 60%
  max-width: 600px
.user-left, .user-right, .user, .header
  display: flex
  flex-direction: row
.rating, .sort
  height: 20px
  width: 20px
.avatar
  height: 50px
  width: 50px
  border-radius: 50%
.user, .header
  width: 100%
  justify-content: space-between
.user-name
  display: flex
  flex-direction: column
.user:nth-of-type(2)
  .avatar
    border: 4px solid yellow
.user:nth-of-type(3)
  .avatar
    border: 4px solid gray
.user:nth-of-type(4)
  .avatar
    border: 4px solid brown
.dimmer
  height: 100vh
  width: 100vw
  background-color: rgba(0,0,0,.4)
  position: absolute
  z-index: 1000
  display: none
.user-modal
  height: 40vh
  width: 50vw
  position: absolute
  z-index: 1001
  background-color: #fff
  display: none
.modal-card
  width: 100%
.modal-right, .modal-card
  display: flex
  flex-direction: column

.modal-header, .modal-body
  width: 100%
  display: flex
  flex-direction: row
  justify-content: space-between
.user-avatar
  height: 60px
  width: 60px
  border-radius: 50%
  border: 2px solid black
</style>
