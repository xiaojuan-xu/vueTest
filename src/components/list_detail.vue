<template>
  <div class="detail">
    <x-table>
      <thead>
        <tr>
          <th>姓名</th>
          <th>时间</th>
          <th>城市</th>
          <th>年龄</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for='d in detail'>
          <td>{{d.name}}</td>
          <td>{{d.time}}</td>
          <td>{{d.city}}</td>
          <td>{{d.age}}</td>
        </tr>
      </tbody>
    </x-table>
  </div>
</template>

<script>
import { XTable } from 'vux'
import Bus from '../eventbus'
export default {
  name: 'detail',
  components: {
    XTable
  },
  data () {
    return {
      detail: []
    }
  },
  created () {
    let vm = this;
    let name = vm.$route.query.name;
    vm.$ajax.get('static/detail.json', {
      params: {
        name: name
      }
    })
    .then(function(res){
      console.log('res: ',res);
      res.data.forEach(function(item, index){
        if(item.name == name){
          console.log('item: ',item);
          item.time = getLocalTime(item.time);
          vm.detail.push(item);
        }
      })
    })
    .catch(function(err){
      console.log('err: ',err);
    })

    Bus.$on('pagedata', data => this.pagedata = data);
    Bus.$on('page', data => this.page = data);
  },
  methods: {},
  beforeDestroy() {
    Bus.$emit('pagedata', this.pagedata);
    Bus.$emit('page', this.page);
  },
  destroyed() {
    Bus.$off('pagedata');
    Bus.$off('page');
  }
} 

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
