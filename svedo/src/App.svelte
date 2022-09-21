<script>
	import ApolloClient from "apollo-boost";
	import { setClient } from "svelte-apollo";
	import Todo from "./Todo.svelte";
  
	export let name;
  
	const client = new ApolloClient({
	  uri: "http://localhost:4000/graphql",
  
	  onError: ({ networkError, graphQLErrors }) => {
		console.log("graphQLErrors", graphQLErrors);
		console.log("networkError", networkError);
	  }
	});
  
	setClient(client);
  </script>
  
  <style>

	h5 {
	  color: grey;
	}
  
	.centrify {
	  text-align: center;
	}
  </style>
  
  <div class="centrify">
	<h5>{name}!</h5>
	<Todo/>
  </div>