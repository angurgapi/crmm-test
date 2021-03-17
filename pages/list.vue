<template>
  <div class="wrapper">
    <div class='users' ref='users'>
      <div class='header'>
          <h2>Рейтинг пользователей</h2>
          <button @click="sortUsers"><img class='sort' src="/img/sort.jpg"></button>
        </div>
       <div class='user' v-for='user in sortedUsers'>
         <div class='user-left'>
           <img class='avatar' :src="`${user.avatar}`">
           <p>{{ user.name }}</p>
         </div>
         <div class="user-right">
          <img class='rating' src="/img/star.png">
           <p>{{ user.rating}}</p>
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

  // mounted(){
  //   this.getUsers()
  // },
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
  //   async sortUsers(){      
  //     try {
  //     return await fetch("https://my-json-server.typicode.com/Vespand/crmm-tasks/users")
  //       .then(res => res.json())
  //       .then(data => {
  //         data.sort((a,b)=>{
  //           return a.rating - b.rating
  //         })
  //         return { users: data }          
  //       });
  //     } catch (e) {
  //     console.error("SOMETHING WENT WRONG :" + e);
  //     }
  //   }
  // }, 
  sortUsers(){
    console.log(this.sortDirection)
    this.sortDirection = this.sortDirection === 'asc' ? 'desc' : 'asc'
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

</style>
