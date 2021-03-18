<template>
  <div class="wrapper">
    <div class="rating">
      <div class='users' ref='users'>
        <div class='header'>
            <p class="bio-header">Рейтинг пользователей</p>
            <button class="button-sort" @click="sortUsers"><img class='sort' src="/img/sort.png"></button>
          </div>

         <div class='user' v-for='user in sortedUsers' @click="userModal(`${user.id}`)">
           <div class='user-left'>
             <img class='avatar' :src="`${user.avatar}`">
             <div class='user-name'>
              <p>Пользователь:</p>
               <p>{{ user.name }}</p>
             </div>
           </div>
           <div class="user-right">
            <img class='user-rating' src="/img/star.png">
             <p>{{ user.rating}}</p>
           </div>
         </div>
      </div>
  </div>
    <div class="dimmer" ref='dimmer'></div>
    
    <div class='user-modal' ref='modal'>
        <div class="modal-card">
          <div class="modal-header">
            <p class='bio-header'>Профиль пользователя</p>
            <div @click='closeModal' class="close-modal"><img class='cross' src="img/close.png"></div>
          </div>          
          <div class="modal-body">
            <div class="modal-left">
              <img class="user-avatar" ref='modalAvatar'>
            </div>
            <div class="modal-right">
              <p>Имя:</p>
              <p class="modal-data" ref="modalName"></p>       
              <p>Баллы:</p>
              <p class="modal-data" ref="modalPoints"></p> 
              <p>Позиция в рейтинге</p>
              <p class="modal-data" ref="modalPosition"></p>     
            </div>
          </div>
          <div class="modal-footer">
            <p>О себе:</p>
            <p class='modal-data' ref='modalDescription'></p>
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
      sortDirection: 'desc',
      winners: ['#FDD835', '#90A4AE', '#795548']
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
      let currentUser = this.users.find(user => {
        return user.id === id
        })
      this.$refs.dimmer.style.display = 'flex'
      this.$refs.modal.style.display = 'flex'
      this.$refs.modalName.textContent = currentUser.name
      this.$refs.modalAvatar.src = currentUser.avatar
      if (this.users.indexOf(currentUser) < 3){
        console.log(this.users.indexOf(currentUser))
        this.$refs.modalAvatar.style.border = "4px solid " + this.winners[this.users.indexOf(currentUser)]
      }
      else{
        this.$refs.modalAvatar.style.border = 'none'
      }
      this.$refs.modalPoints.textContent = currentUser.rating
      this.$refs.modalDescription.textContent = currentUser.description
      this.$refs.modalPosition.textContent = this.users.indexOf(currentUser) + 1
    },
    closeModal(){
      this.$refs.dimmer.style.display = 'none'
      this.$refs.modal.style.display = 'none'
    }
  }
};
</script>
<style lang="sass">
@font-face
    font-family: 'Avenir'
    font-style: normal
    font-weight: 600
    src: url('~assets/fonts/avenir-light.ttf') format('truetype')
.rating
  width: 960px
  color: #2C3E50
  background-color: #FAFAFA
.users
  display: flex
  flex-direction: column
  justify-content: center
  align-items: center
  width: 600px
  margin: auto
.user-left, .user-right, .user, .header
  display: flex
  flex-direction: row
  align-items: center
.button-sort
  height: 34px
  width: 34px
  background-color: #FAFAFA
  border: 1px solid #2C3E50
  border-radius: 5px
  img
    height: 12px
    width: 18px
.button-sort:hover
  background-color: rgba(0, 0, 0, 0.07)
.user-right
  width: 50px
  height: 24px
  justify-content: space-between
  margin-right: 14px
.user-rating
  height: 20px
  width: 20px
.avatar
  height: 60px
  width: 60px
  border-radius: 50%
  margin: 14px
.header
  width: 584px
  height: 34px
  margin-top: 15px
  justify-content: space-between
.user
  width: 100%
  height: 88px
  justify-content: space-between
  align-items: center
.user:hover
  background: rgba(0, 0, 0, 0.07)
  border-radius: 10px
.user-name
  display: flex
  flex-direction: column
  font-family: 'Avenir'

.user:nth-of-type(2)
  .avatar
    border: 4px solid #FDD835
.user:nth-of-type(3)
  .avatar
    border: 4px solid #90A4AE
.user:nth-of-type(4)
  .avatar
    border: 4px solid #795548
.dimmer
  height: 100%
  width: 100%
  background-color: rgba(0, 0, 0, 0.07)
  position: fixed
  bottom: 0
  z-index: 1000
  display: none
.user-modal
  font-family: 'Avenir'
  line-height: 18px
  color: #2C3E50
  width: 478px
  position: fixed
  top: 128px
  z-index: 1001
  background-color: #fff
  display: none
.modal-card
  width: 100%
  padding: 20px
.close-modal
  height: 32px
  width: 32px
  display: flex
  justify-content: center
  align-items: center
  img
    height: 15px
    width: 15px
.close-modal:hover
  border-radius: 50%
  background-color: rgba(0, 0, 0, 0.07)
.modal-right, .modal-card
  display: flex
  flex-direction: column
.modal-body
  margin-top: 29px
.modal-right
  width: 60%
.modal-header, .modal-body
  align-items: center
  width: 100%
  display: flex
  flex-direction: row
  justify-content: space-between
.modal-right
  margin-left: 37px
.user-avatar
  height: 129px
  width: 129px
  border-radius: 50%
.bio-header
  font-size: 24px
  font-weight: 400

.modal-footer
  margin-top: 17px
.modal-data
  font-size: 16px
</style>
