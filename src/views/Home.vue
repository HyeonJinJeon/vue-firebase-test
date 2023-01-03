<template>
  <div class="home">
    <table border="1">
      <th>이름</th>
      <th>나이</th>
      <th>성별</th>
      <th>id</th>
      <th>정보 확인</th>
      <tr v-for="(rows,i) in rows" :key="i">
        <td>{{rows.name}}</td>
        <td>{{rows.age}}</td>
        <td>{{rows.gender}}</td>
        <td>{{rows.id}}</td>
        <button @click="goUrl(i)">상세정보</button>
      </tr>
    </table>
    {{high}}
  </div>
</template>


<script>
// @ is an alias to /src
import {firebase} from '@/firebase/firebaseConfig';


export default {
  name: 'home',
  components: {

  },
  data(){
    return {
      fbCollection: 'users',
      user: 'rows.id',
      rows: [],
      high: '',
    }
  },
  methods: {
    getDataList(){
      const self = this;
      const db = firebase.firestore();
      db.collection(self.fbCollection)
          .get()
          .then((querySnapshot) => {
            if (querySnapshot.size === 0) {
              return
            }
            querySnapshot.forEach((doc) => {
              const _data = doc.data();
              // const _user = doc.data().doc(self._data.id);
              _data.id = doc.id
              console.log(_data)
              self.rows.push(_data);
            });
          })
    },
    goUrl(i) {
      this.$router.push({name: 'eachuserdata', params: {id: this.rows[i].id}}).catch(()=>{});
    },
    init(){
      this.getDataList()
    }
  },
  mounted() {
    const self = this;
    self.init();
  },
  comments: {

  }
}
</script>
