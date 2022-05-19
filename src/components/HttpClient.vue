<template>

  <section class="src-componentes-http-client">
     <div class="jumbotron">
      <h2>Componente HttpClient</h2>
      <hr>
      <hr>
      <br>

      <button class="btn btn-warning my-3 mr-3" @click="getPostsXHRcb()">Pedir XHR (cb)</button>
      <button class="btn btn-success my-3 mr-3" @click="getPostsXHRpromise()">Pedir XHR (promise)</button>
      <button class="btn btn-success my-3 mr-3" @click="getPostsFetch()">Pedir Fetch </button>
      <button class="btn btn-success my-3 mr-3" @click="getPostsAxios()">Pedir Axios </button>
      <button class="btn btn-danger my-3" @click="posts=[]">CLEAR</button>
      <br>
      <div v-if="posts.length" class="table-responsive">
        <table class="table table-dark">
          <tr>
            <th>id</th>
            <th>Nombre</th>
            <th>Email</th>
            <th>Teléfono</th>
          </tr>
          <tr v-for="(post, index) in posts" :key="index">
              <td>{{ post.id }}</td>
              <td>{{ post.nombre }}</td>
              <td>{{ post.email }}</td>
              <td>{{ post.numero_telefono }}</td>
          </tr>
        </table>
        <h5 class="alert alert-primary">Se encontraron {{posts.length}} posts.</h5>
      </div>
      <h4 v-else class="alert alert-danger text-center">No se encontraron posts</h4>

     </div>
  </section>

</template>

<script>

  export default  {
    name: 'src-componentes-http-client',
    props: [],
    mounted () {

    },
    data () {
      return {
        url: 'https://62857423f0e8f0bb7c04308c.mockapi.io/usuarios',
        posts: []
      }
    },
    methods: {
      wrapperXHRpromise() {
        return new Promise((resolve,reject) => {
         
          const xhr = new XMLHttpRequest()
          xhr.open('get', this.url)

          xhr.addEventListener('load', () => {
            if(xhr.status == 200) {
              let respuesta = JSON.parse(xhr.response)
              //console.log(respuesta)
              resolve(respuesta)
            }
            else {
              console.error('ERROR XHR cb (status)', xhr.status)
              let error = {
                title: 'ERROR XHR cb (status)',
                status: xhr.status
              }
              reject(error)
            }
          })
          xhr.addEventListener('error', e => {
              console.error('ERROR XHR cb (ajax)', e)
              let error = {
                title: 'ERROR XHR cb (ajax)',
                info: e
              }
              reject(error)
          })
          xhr.send()
        })
      },
      getPostsXHRcb() {
          const xhr = new XMLHttpRequest()
          xhr.open('get', this.url)
          xhr.addEventListener('load', () => {
            if(xhr.status == 200) {
              let respuesta = JSON.parse(xhr.response)
              this.posts = respuesta
            }
            else {
              console.error('ERROR XHR cb (status)', xhr.status)
            }
          })
          xhr.addEventListener('error', e => {
              console.error('ERROR XHR cb (ajax)', e)
          })
          xhr.send()
      },
      async getPostsXHRpromise() {
          try {
            let respuesta = await this.wrapperXHRpromise()
            this.posts = respuesta
          }
          catch(error) {
            console.error('Error XHRpromise', error)
          }
      },
      async getPostsFetch() {

        try {
          let response = await fetch(this.url)
          let respuesta = await response.json()
          this.posts = respuesta
        }
        catch(error) {
          console.error('Error Fetch', error)
        }
      },
      async getPostsAxios() {
        try {
          let { data } = await this.axios(this.url)
          this.posts = data
          console.log(this.posts);
        }
        catch(error) {
          console.error('Error Axios', error)
        }   
      }
    },
    computed: {
    }
}
</script>
<style scoped lang="css">
  .jumbotron {
    background-color: teal;
    color: white;
  }
  hr {
    background-color: #bbb;
  }  
</style>
