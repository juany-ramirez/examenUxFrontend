<template>
	<div class="container">
		<br><br>
		<table class="table centered">
			<thead>
				<tr>
					<th>Nombre</th>
					<th>Dificultad</th>
					<th>Aprendizaje</th>
					<th>Modificar</th>
					<th>Borrar</th>
				</tr>
			</thead>
			<tbody>
				<tr v-for="hechizo in hechizos">
					<td>{{hechizo.nombre}}</td>
					<td>{{hechizo.dificultad}}</td>
					<td>{{hechizo.aprendizaje}}</td>
					<td>
						<a v-on:click="startToModifyBebida(hechizo)" class="btn-floating btn-small waves-effect waves-light grey darken-4"><i class="material-icons">arrow_downward</i></a>
					</td>
					<td>
						<a v-on:click="deleteHechizo(hechizo._id)" class="btn-floating btn-small waves-effect waves-light red"><i class="material-icons">delete</i></a>
					</td>
				</tr>
			</tbody>
		</table>
		<br>
		<ul id="tabs-swipe-demo" class="tabs">
	    <li class="tab col s3"><a class="active" href="#test-swipe-1">Crear</a></li>
	    <li class="tab col s3"><a  href="#test-swipe-2">Modificar</a></li>
	  </ul>

	  <div id="test-swipe-1" class="col s12">
			<div class="row">
	        <div class="input-field col s6">
	          <input type="text" v-model="hechizo.nombre" :disabled="loading"  id="Nombre">
	          <label for="Nombre">Nombre</label>
	        </div>
					<div class="input-field col s6">
	          <input v-model="hechizo.dificultad" :disabled="loading" id="Dificultad" type="text"   class="validate">
	          <label for="Dificultad">Dificultad</label>
	        </div>
					<div class="input-field col s6">
	          <input v-model="hechizo.aprendizaje" :disabled="loading"  id="Aprendizaje" type="number" class="validate">
	          <label for="Aprendizaje">Aprendizaje</label>
	        </div>
	      </div>
				<a class="waves-effect waves-light btn-large" v-on:click="createHechizo" :disabled="loading" id="boton">
					<i class="material-icons left">create</i>Crear
				</a>

		</div>
	  <div id="test-swipe-2" class="col s12">

				<a class="waves-effect waves-light btn-large" v-on:click="createHechizo" :disabled="loading" id="boton">
					<i class="material-icons left">update</i>Update</a>

		</div>

	</div>
</template>

<script>
	import baseUrl from '../../config'
	export default{
		name: 'home',
		data(){
			return{
				hechizos: [],
				hechizo:{
				},
				loading: false
			}
		},
		methods: {
			getHechizos(){
				this.$http.get(`${baseUrl.uri}/hechizos`).then((response)=>{
					this.hechizos=response.body;
				});
			},
			createHechizo(){
				this.loading=true;
				this.$http.post(`${baseUrl.uri}/hechizos/create`,this.hechizo)
				.then((response)=>{
					this.loading=false;
					if(response.body.success){
						swal("Creado con exito!", "Los cambios estan en la tabla", "success");
						this.getHechizos();
					}else{
						sweetAlert("Oops...", "Error al crear", "error");
						this.getHechizos();
					}
				});
			},
			modifyHechizo(id){
				this.loading=true;
				this.$http.update(`${baseUrl.uri}/hechizos/update/`+identidad,this.hechizo)
				.then((response)=>{
					this.loading=false;
					if(response.body.success){
						swal("Creado con exito!", "Los cambios estan en la tabla", "success");
						this.getHechizos();
					}else{
						sweetAlert("Oops...", "Error al crear", "error");
					}
				});
			},
			deleteHechizo(id){
					this.loading=true;
					this.$http.delete(`${baseUrl.uri}/hechizos/delete/`+id)
						.then((response)=>{
						this.loading=false;
						if(response.body.success){
							this.getHechizos();
							swal("Deleted!", "Se ha eliminado el Libro", "success");
						}else{
							sweetAlert("Oops...", "Error al crear", "error");
							this.getHechizos();
						}
					});
			},
			startToModifyBebida(hechizo1){

			}
		},
		beforeMount(){
			this.getHechizos();
		},
		mounted(){
			$('ul.tabs').tabs();
			$('select').material_select();
		}
	}
</script>

<style scoped>
		h2{
			font-family: 'Passion One', cursive;

		}
			#test-swipe-1{
				/*background-color: #E6E2AF;*/
				color: #4C1B1B;
				padding-left: 50px;
				padding-right: 50px;
				padding-top: 20px;
				padding-bottom: 30px;
				text-align: center;
			}
			#test-swipe-2{
				/*background-color: #F6E497;*/
				color: #4C1B1B;
				padding-left: 50px;
				padding-right: 50px;
				padding-top: 20px;
				padding-bottom: 30px;
				text-align: center;
			}
			#test-swipe-3{
				/*background-color: #E6E2AF;*/
				color: #4C1B1B;
				padding-left: 50px;
				padding-right: 50px;
				padding-top: 20px;
				padding-bottom: 30px;
				text-align: center;
			}
			body{
				font-family: 'Libre Franklin', sans-serif;
				font-size: 20px;
			}
		.tabs .indicator {
			background-color: #A7A37E !important;
			color: #4C1B1B !important;
		}
		.tabs {
			background-color: #B9121B !important;
			color: #4C1B1B !important;
			font-family: 'Spectral', serif;
			font-weight: bold;
			border-radius: 30px;
		}

		.tabs .tab a.active {
		  color: white;
		}

		.tabs .tab a:hover {
		  color: gray;
		}

		.tabs .tab a {
		  color: #4C1B1B;
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
			background-color: #B8630B;
		}
		th {
		  color:#D5DDE5;;
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
