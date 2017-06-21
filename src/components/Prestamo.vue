<template>
	<div class="container">
		<br><br>
    <div class="row center">
			<div class="col s8">
        <nav >
         <div class="nav-wrapper">
           <form>
             <div class="input-field">
               <input  v-on:keyup.enter="search()" v-model="searchText" id="search" type="search" required>
               <label class="label-icon" for="search"><i class="material-icons">search</i></label>
               <i class="material-icons">close</i>
             </div>
           </form>
         </div>
       </nav>
      </div>
			<div class="col s2">
        <div class="input-field">
          <select id="searchFilter" v-model="elElegido">
            <option value="1">Titulo</option>
            <option value="2">Id</option>
            <option value="3">Genero</option>
            <option value="4">Autor</option>
            <option value="5">Key</option>
          </select>
          <label>Escoger Busqueda</label>
        </div>
      </div>
			<div class="col s2">
				<a class="waves-effect waves-light btn-large" v-on:click="search" :disabled="loading" id="boton">
	        <i class="material-icons left">search</i></a>
			</div>
    </div>
		<table class="table centered">
			<thead>
				<tr>
					<th>ID</th>
					<th>username</th>
					<th>ocupacion</th>
					<th>nacimiento</th>
					<th>creencias</th>
					<th>Add</th>
				</tr>
			</thead>

			<tbody>
				<tr v-for="user in users">
					<td>{{user._id}}</td>
					<td>{{user.username}}</td>
					<td>{{user.ocupacion}}</td>
					<td>{{user.nacimiento}}</td>
					<td>{{user.creencias}}</td>
					<td>{{user.copias_disponible}}</td>
					<td>
						<a v-on:click="agregarAmigo(user._id)" class="btn-floating btn-small waves-effect waves-light red"><i class="material-icons">add</i></a>
					</td>
				</tr>
			</tbody>
		</table>
		<br>
	</div>
</template>

<script>
	import baseUrl from '../../config'
	export default{
		name: 'prestamo',
		data(){
			return{
				users: [],
				elElegido: 1,
				user:{
				},
				searchText:'',
				loading: false
			}
		},
		methods: {
			getUsers(){
				this.$http.get(`${baseUrl.uri}/users`).then((response)=>{
					this.users=response.body;
				});
			},
			agregarAmigo(id){
				this.loading=true;
				this.$http.put(`${baseUrl.uri}/users/borrowuser/`+id)
				.then((response)=>{
						this.loading=false;
						if(response.body === 'updated succesfully'){
							this.getUsers();
							swal("Se ha realizado el prestamo!", "Los cambios estan en la tabla", "success");
						}else{
							sweetAlert("Oops...", response.body, "error");
						}
					});
			},
			search(){
				console.log(this.elElegido);
				console.log(this.searchText);
				if(this.searchText===''){
					this.$http.get(`${baseUrl.uri}/users`).then((response)=>{
				this.loading=false;
						this.users=response.body;
					});
				}else if(elElegido ===1){
						this.$http.get(`${baseUrl.uri}/users/searchbyname/`+searchText).then((response)=>{
							this.users=response.body;
						});
						searchText='';
				}else if(elElegido ===2){
						this.$http.get(`${baseUrl.uri}/users/searchbyid/`+searchText).then((response)=>{
							this.users=response.body;
						});
						searchText='';
				}else if(elElegido ===3){
						this.$http.get(`${baseUrl.uri}/users/searchbygenre/`+searchText).then((response)=>{
							this.users=response.body;
						});
						searchText='';
				}else if(elElegido ===4){
						this.$http.get(`${baseUrl.uri}/users/searchbyauthor/`+searchText).then((response)=>{
							this.users=response.body;
						});
						searchText='';
				}else if(elElegido ===5){
						this.$http.get(`${baseUrl.uri}/users/searchbykey`,searchText).then((response)=>{
							this.users=response.body;
						});
						searchText='';
				}

			}
		},
		watch: {
			elElegido(val){
				console.log(val);
			}
		},
		beforeMount(){
			this.getUsers();
		},
		mounted(){
			$('#searchFilter').material_select();
		}
	}
</script>

<style scoped>
    .prestamos{
        padding: 5%;
        align-items: center;
        text-align: center;
        background-color: #B9121B;
				border-radius: 10px;
    }
    .nav-wrapper{
      background-color: #BD8D46;
      font-family: 'Spectral', serif;
      font-size: 65px;
    }
		h2{
			font-family: 'Passion One', cursive;

		}
			body{
				font-family: 'Libre Franklin', sans-serif;
				font-size: 20px;
			}

		.table thead{
			font-family: 'Josefin Slab', serif;
			font-weight: bold;
			font-size: 30px;
		}
		.table{
			font-family: 'Spectral', serif;
			font-size: 17px;

			background: white;
		  border-radius:3px;
		  border-collapse: collapse;
		  height: 320px;
		  padding:5px;
		  width: 100%;
		  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.1);
		  animation: float 5s infinite;

		}
		#boton{
			background-color: #1b1e24;
		}
		th {
		  color:#D5DDE5;
		  background:#1b1e24;
		  border-bottom:4px solid #9ea7af;
		  border-right: 1px solid #343a45;
		  font-size:23px;
		  font-weight: 100;
		  padding:24px;
		  text-align:left;
		  text-shadow: 0 1px 1px rgba(0, 0, 0, 0.1);
		  vertical-align:middle;
		}

		/* label focus color */

		.input-field input:focus + label {
			color: #4C1B1B !important;
		}
		/* label underline focus color */
		.row .input-field input:focus {
			border-bottom: 1px solid #4C1B1B !important;
			box-shadow: 0 1px 0 0 #4C1B1B !important
		}
		.input-field input:focus + label {
			color: #4C1B1B !important;
		}
		/* label underline focus color */
		.row .input-field input:focus {
			border-bottom: 1px solid #4C1B1B !important;
			box-shadow: 0 1px 0 0 #4C1B1B !important
		}
		.input-field textarea:focus + label {
			color: #4C1B1B !important;
		}
		/* label underline focus color */
		.row .input-field textarea:focus {
			border-bottom: 1px solid #4C1B1B !important;
			box-shadow: 0 1px 0 0 #4C1B1B !important
		}
</style>
