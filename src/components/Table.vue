<template>
	<div>
		<div class="container center">
		<table id="tableData" class="table-striped table-bordered table-hover">
			<thead>
				<tr>
					<th v-for="key in sort_column" @click="sortBy(key)" :class="{ active: sortKey == key }">{{ key | capitalize }}
					<span class="arrow" :class="sortOrders > 0 ? 'asc' : 'dsc'"></span>
					</th>
				</tr>
				<!--
				<tr>
					<th @click="sortBy('name')">Name</th>
					<th @click="sortBy('name')">Age</th>
				</tr>
				-->
			</thead>
			<tbody>
				<tr v-for="row in paginateTable">
					<td v-for="key in sort_column">
						{{ row[key] }}
					</td>
					<!--
					<td>{{ row.name }}</td>
					<td>{{ row.age }}</td>
					-->
				</tr>
			</tbody>
		</table>
		</div>
		<br>
		<form>
			<input name="query" v-model="searchQuery" placeholder="Search">
		</form>
		<br>
		<!--
		<ul>
			<li v-for="pageNumber in totalPages" v-if="Math.abs(pageNumber - currentPage) < 3 || pageNumber == totalPages - 1 || pageNumber == 0">
				<a href="#" @click="setPage(pageNumber)" :class="{current: currentPage === pageNumber, last: (pageNumber == totalPages - 1 && Math.abs(pageNumber - currentPage) > 3), first: (pageNumber == 0 && Math.abs(pageNumber - currentPage) > 3)}"> {{ pageNumber }}</a>
			</li>
		</ul>
		--> 
		<div class="row" id="btnContainer">
			<div class="col-md-3"><a href="#" @click="prevPage" v-for="pageNumber in totalPages" v-if="Math.abs(pageNumber - currentPage) < 2 || pageNumber == totalPages - 1 || pageNumber == 0">Prev</a></div>
			<div class="col-md-3">{{ currentPage+1 }} of {{ totalPages }}</div>
			<div class="col-md-3"><a href="#" @click="nextPage">Next</a></div>
			<!-- for before pagination was working - to clean up console logs
			<a >Prev</a>
			<a >Next</a>
			-->
			<!--
			To show what is being entered in the search field
			<div>{{ searchQuery }}</div>
			-->
		</div>
		<br>
		<!--
		<div class="row" id="test1">
				<div class="span4" style="background-color:yellow;"><p>Test 1</p></div>
				<div class="span4" style="background-color:pink;"><p>Test 2</p></div>
				<div class="span4" style="background-color:blue;"><p>Test 3</p></div>
		</div>
		-->
	</div>
</template>

<script>
	export default {
		name: 'TableData',
		data () {
			var sortOrders = 1
			var table_data = [
					{name: 'John', 'age': 24},
					{name: 'Robbie', 'age': 30},
					{name: 'Richard', 'age': 31},
					{name: 'Tim', 'age': 28}
				]
			var sort_column = ['name', 'age']
			var page_num = 0
			var currentPage = 0
			var itemsPerPage = 2
			var resultCount = 0
			//this.sort_column.forEach(function (key) {
			//	sortOrders[key] = 1
			//})
			return {
				//returning the table we created in data. in future, can import bigger table from
				//another file
				table_data,
				//text to search on
				searchQuery: "",

				//column to sort by - created earlier in data
				sort_column,

				//current page number
				page_num,

				//referencing the above column sorting function in data
				sortKey: '',
				sortOrders,
				currentPage,
				itemsPerPage,
				resultCount,
				page_num
				
			}
		},
		computed: {
			filteredTable: function() {
				var filterKey = this.searchQuery.toLowerCase()
				var data = this.table_data
				if (filterKey) {
					data = data.filter(function (row) {
						return Object.keys(row).some(function (key) {
							return String(row[key]).toLowerCase().indexOf(filterKey) > -1
						})
					})
				}
				return data
			},
			sortedTable: function() {
				var data = this.filteredTable
				var sortKey = this.sortKey
				var order = this.sortOrders || 1
				if (sortKey) {
					data = data.slice().sort(function (a, b) {
						a = a[sortKey]
						b = b[sortKey]
						return (a === b ? 0 : a < b ? -1 : 1) * order
					})
				}
				return data 
			},
			totalPages: function () {
				console.log("totalPages resultCount " + this.resultCount)
				console.log("totalPages currentPage " + this.currentPage)
				console.log("totalPages " + Math.ceil(this.resultCount / this.itemsPerPage))
				return Math.ceil(this.resultCount / this.itemsPerPage)
			},
			paginateTable: function () {
				console.log("paginateTable started")
				var data = this.sortedTable
				var test = 1
				this.resultCount = data.length
				if (this.currentPage >= this.totalPages) {
					this.currentPage = this.totalPages - 1
					console.log("paginateTable adjusted currentPage")
				}
				var index = this.currentPage * this.itemsPerPage
				return data.slice(index, index + this.itemsPerPage)
			}						
		},
		filters: {
			capitalize: function (str) {
      			return str.charAt(0).toUpperCase() + str.slice(1)
    		},
    		paginateTable2nd: function (list) {
				console.log("paginateTable Filter started")
				this.resultCount = list.length
				if (this.currentPage >= this.totalPages) {
					this.currentPage = this.totalPages - 1
					console.log("paginateTable adjusted currentPage")
				}
				var index = this.currentPage * this.itemsPerPage
				return list.slice(index, index + this.itemsPerPage)
			}
		},
		methods: {
			sortBy: function (key) {
				if (this.sortKey == key) {
					this.sortOrders = this.sortOrders * -1
				}
				this.sortKey = key
			},
			totalPages2nd: function () {
				console.log("Method totalPages resultCount " + this.resultCount)
				console.log("Method totalPages currentPage " + this.currentPage)
				console.log("Method totalPages " + Math.ceil(this.resultCount / this.itemsPerPage))
				return Math.ceil(this.resultCount / this.itemsPerPage)
			},
			setPage: function (pageNumber) {
				console.log("setPage pageNumber " + pageNumber)
				this.currentPage = pageNumber
				console.log("setPage activated")
			},
			nextPage: function () {
				console.log("nextPage currentPage " + this.currentPage)
				this.currentPage += 1
				console.log("nextPage currentPage-2 " + this.currentPage)
			},
			prevPage: function () {
				console.log("prevPage currentPage " + this.currentPage)
				if (this.currentPage !== 0) {
					this.currentPage -= 1
				}

				console.log("prevPage currentPage-2 " + this.currentPage)
			}
		}
	}
</script>

<style>
	table {
		font-size: 16px;
		font-family: Helvetica Neue, Arial, sans-serif;
		border: 2px solid #42b983;
		background-color: #fff;
		border-radius: 3px;
	}
	th {
		background-color: #42b983;
	  	color: rgba(255,255,255,0.66);
	  	cursor: pointer;
	  	user-select: none;
	}
	th, td {
  		min-width: 120px;
  		padding: 10px 20px;
	}
	tr {
		padding: 50px;
	}
	th.active {
  		color: #fff;
	}

	th.active .arrow {
	  opacity: 1;
	}
	.arrow {
	  display: inline-block;
	  vertical-align: middle;
	  width: 0;
	  height: 0;
	  margin-left: 5px;
	  opacity: 0.66;
	}

	.arrow.asc {
	  border-left: 4px solid transparent;
	  border-right: 4px solid transparent;
	  border-bottom: 4px solid #fff;
	}

	.arrow.dsc {
	  border-left: 4px solid transparent;
	  border-right: 4px solid transparent;
	  border-top: 4px solid #fff;
	}

	a {
		color: #999;
	}

	.current {
		color: red;
	}

	ul {
		padding: 0;
		list-style-type: none;
	}

	li {
		display: inline;
		margin: 5px 5px;
	}

	a.first::after {
		content: '...'
	}

	a.last::before {
		content: '...'
	}
	.container {
		width: 100%;
		float: left;
		border: none;
		text-align: center;
	}
	.container button {
		display: inline-block;
		border: solid 2px;

	}
	#tableData {
		margin: 0px auto;
		float: none;
	}
	#btn1 {
		float: left;
		text-align: right;
		border: solid 2px;
	}
	#btn2 {
		float: center;
		text-align: center;
		border: solid 2px;
		width: 150px;
	}
	#btn3 {
		float: right;
		text-align: left;
		border: solid 2px;
	}
	#btnContainer {
		border: solid 2px;
		width: 400px;
		display: inline-block;

	}
	#test1 {
		display: inline-grid;
	}
</style>