<template>
	<div>
		<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
		<table class="table-striped table-bordered table-hover">
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
		<br>
		<form>
			<input name="query" v-model="searchQuery" placeholder="Search">
		</form>
		<br>
		<ul>
			<li v-for="pageNumber in totalPages" v-if="Math.abs(pageNumber - currentPage) < 3 || pageNumber == totalPages - 1 || pageNumber == 0">
				<a href="#" @click="setPage(pageNumber)" :class="{current: currentPage === pageNumber, last: (pageNumber == totalPages - 1 && Math.abs(pageNumber - currentPage) > 3), first: (pageNumber == 0 && Math.abs(pageNumber - currentPage) > 3)}"> {{ pageNumber }}</a>
			</li>
		</ul>
		<!-- For when we get to the Paginate section
		<a @click="prevPage">Prev</a>
		<a @click="nextPage">Next</a>
		-->
		<!-- for before pagination was working - to clean up console logs
		<a >Prev</a>
		<a >Next</a>
		-->
		<!--
		To show what is being entered in the search field
		<div>{{ searchQuery }}</div>
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
</style>