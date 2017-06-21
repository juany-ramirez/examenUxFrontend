<template>
	<div class="container">
		<br><br>
		<table class="table centered">
			<thead>
				<tr>
					<th>Título</th>
					<th>Género</th>
					<th>Autor</th>
					<th>Publicación</th>
					<th>Editorial</th>
					<th>Descripción</th>
					<th>Borrar</th>
				</tr>
			</thead>
			<tbody>
				<tr v-for="book in books">
					<td>{{book.titulo}}</td>
					<td>{{book.genero}}</td>
					<td>{{book.autor}}</td>
					<td>{{book.publicacion}}</td>
					<td>{{book.editorial}}</td>
					<td>{{book.descripcion}}</td>
					<td>
						<a v-on:click="deleteBook(book._id)" class="btn-floating btn-small waves-effect waves-light red"><i class="material-icons">delete</i></a>
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
	          <input type="text" v-model="book.titulo" :disabled="loading"  id="Titulo">
	          <label for="Titulo">Titulo</label>
	        </div>
					<div class="input-field col s6">
	          <input v-model="book.genero" :disabled="loading" id="Genero" type="text"   class="validate">
	          <label for="Genero">Genero</label>
	        </div>
					<div class="input-field col s6">
	          <input v-model="book.autor" :disabled="loading"  id="Autor" type="text" class="validate">
	          <label for="Autor">Autor</label>
	        </div>
					<div class="input-field col s6">
	          <input type="number" v-model="book.publicacion" :disabled="loading"  id="Publicación">
	          <label for="Publicación">Publicación</label>
	        </div>
					<div class="input-field col s6">
	          <input v-model="book.editorial" :disabled="loading" id="Editorial" type="text"  class="validate">
	          <label for="Editorial">Editorial</label>
	        </div>
					<div class="row">
				    <form class="col s12">
				      <div class="row">
				        <div class="input-field col s12">
				          <textarea v-model="book.descripcion" :disabled="loading"  id="descripcion" type="text"  class="materialize-textarea"></textarea>
				          <label for="descripcion">Descripción</label>
				        </div>
				      </div>
				    </form>
				  </div>
					<div class="input-field col s6">
	          <input v-model="book.keywords" :disabled="loading" id="Keywords" type="text"  class="validate">
	          <label for="Keywords">Keywords</label>
	        </div>
					<div class="input-field col s6">
	          <input v-model="book.copias_total" :disabled="loading"  id="Total de Copias" type="number" class="validate">
	          <label for="Total de Copias">Total de Copias</label>
	        </div>
					<div class="input-field col s6">
	          <input v-model="book.copias_disponible" :disabled="loading"  id="Copias Disponibles" type="number" class="validate">
	          <label for="Copias Disponibles">Copias Disponibles</label>
	        </div>
	      </div>
				<a class="waves-effect waves-light btn-large" v-on:click="createBook" :disabled="loading" id="boton">
					<i class="material-icons left">create</i>Crear
				</a>

		</div>
	  <div id="test-swipe-2" class="col s12">

			<div class="input-field col s12">
			 <select v-model="selected" >
				 <option v-for="book in books" v-bind:value="book"value="1"> {{book.titulo}} </option>
			 </select>
			 <label>Selected: {{ selected }}</label>
		 </div>
		 <div class="row">
				 <div class="input-field col s6">
					 <input type="text" v-model="book.titulo" :disabled="loading"  id="Titulo1">
					 <label for="Titulo1">Titulo</label>
				 </div>
				 <div class="input-field col s6">
					 <input v-model="book.genero" :disabled="loading" id="Genero1" type="text"   class="validate">
					 <label for="Genero1">Genero</label>
				 </div>
				 <div class="input-field col s6">
					 <input v-model="book.autor" :disabled="loading"  id="Autor1" type="text" class="validate">
					 <label for="Autor1">Autor</label>
				 </div>
				 <div class="input-field col s6">
					 <input type="number" v-model="book.publicacion" :disabled="loading"  id="Publicación1">
					 <label for="Publicación1">Publicación</label>
				 </div>
				 <div class="input-field col s6">
					 <input v-model="book.editorial" :disabled="loading" id="Editorial1" type="text"  class="validate">
					 <label for="Editorial1">Editorial</label>
				 </div>
				 <div class="row">
					 <form class="col s12">
						 <div class="row">
							 <div class="input-field col s12">
								 <textarea v-model="book.descripcion" :disabled="loading"  id="descripcion1" type="text"  class="materialize-textarea"></textarea>
								 <label for="descripcion1">Descripción</label>
							 </div>
						 </div>
					 </form>
				 </div>
				 <div class="input-field col s6">
					 <input v-model="book.keywords" :disabled="loading" id="Keywords1" type="text"  class="validate">
					 <label for="Keywords1">Keywords</label>
				 </div>
				 <div class="input-field col s6">
					 <input v-model="book.copias_total" :disabled="loading"  id="Total de Copias1" type="number" class="validate">
					 <label for="Total de Copias1">Total de Copias</label>
				 </div>
				 <div class="input-field col s6">
					 <input v-model="book.copias_disponible" :disabled="loading"  id="Copias Disponibles1" type="number" class="validate">
					 <label for="Copias Disponibles1">Copias Disponibles</label>
				 </div>
			 </div>
				<a class="waves-effect waves-light btn-large" v-on:click="createBook" :disabled="loading" id="boton">
					<i class="material-icons left">update</i>Update</a>

		</div>

	</div>
</template>

<script>
	export default{
		name: 'home',
		data(){
			return{
				books: [],
				book:{
				},
				loading: false
			}
		},
		methods: {
			getBooks(){
				this.$http.get('http://localhost:8000/books').then((response)=>{
					this.books=response.body;
				});
			},
			createBook(){
				this.loading=true;
				this.$http.post('http://localhost:8000/books/create',this.book)
				.then((response)=>{
					this.loading=false;
					if(response.body.success){
						swal("Creado con exito!", "Los cambios estan en la tabla", "success");
						this.getBooks();
					}else{
						sweetAlert("Oops...", "Error al crear", "error");
					}
				});
			},
			modifyBook(id){
				this.loading=true;
				this.$http.update('http://localhost:8000/books/update/'+identidad,this.book)
				.then((response)=>{
					this.loading=false;
					if(response.body.success){
						swal("Creado con exito!", "Los cambios estan en la tabla", "success");
						this.getBooks();
					}else{
						sweetAlert("Oops...", "Error al crear", "error");
					}
				});
			},
			deleteBook(id){
					this.loading=true;
					this.$http.delete('http://localhost:8000/books/delete/'+id)
						.then((response)=>{
						this.loading=false;
						if(response.body.success){
							this.getBooks();
							swal("Deleted!", "Se ha eliminado el Libro", "success");
						}else{
							sweetAlert("Oops...", "Error al crear", "error");
						}
					});
					// swal({
					//   title: "Are you sure?",
					//   text: "You will not be able to recover this book!",
					//   type: "warning",
					//   showCancelButton: true,
					//   confirmButtonColor: "#DD6B55",
					//   confirmButtonText: "Yes, delete it!",
					//   cancelButtonText: "No, cancel!",
					//   closeOnConfirm: false,
					//   closeOnCancel: false
					// },
					// function(isConfirm){
					//   if (isConfirm) {
					//     swal("Deleted!", "Se ha eliminado el Libro", "success");
					//
					// 	} else {
					//     swal("Cancelled", "No se ha eliminado nada", "error");
					//   }
					// });
			}
		},
		beforeMount(){
			this.getBooks();
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
