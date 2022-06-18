<script>
	import Breadcrumb from './Breadcrumb.svelte';
	import List from './List.svelte';
	import Nodes from './Nodes.svelte';
	import CallFunction from './Nodes/CallFunction.svelte';
	import Print from './Nodes/Print.svelte';
	import SetVariable from './Nodes/SetVariable.svelte';

	// This is the array of the components that are rendered
	let list = [];

	// For testing; this would be GETted from an external database later
	const saved = [
		['Function_Call', {'name': 'something', 'arguments': 'arg: myvariable'}],
		['Variable_Assignment', {'name': 'myvariable', 'value': '344'}],
		['Print', {'value': 'hello, world!'}]
	]

	// The current state of the list, used to auto save
	// <main> is the main function, and other nodes may be added. (nodeName: [instructions])
	let currentState = {'<main>': [],};
	let currentNode = '<main>'; // The node that is being edited
	let hidden = true;

	const textToNode = {
		'Function_Call': CallFunction,
		'Variable_Assignment': SetVariable,
		'Print': Print
	}

	// I do not know why or what this is
	const sortList = ev => {list = ev.detail};

	function compile() {
		for (const child of document.getElementById('code').children) {
			for (const subchild of child.children[0].children) {
				if (subchild.tagName == 'TYPE') 
					console.log('type:', subchild.innerHTML);
				else 
					console.log(`${subchild.getAttribute('is')}: ${subchild.tagName == 'DIV' ? subchild.innerHTML : subchild.value}`);
			}
		}
	}

	function load() {
		document.getElementById('code').innerHTML = '';

		for (const node of saved) {
			list = [...list, {id: list.length + 1, component: textToNode[node[0]]}];
			currentState[currentNode].push(node);
		}
	}

	function hydrate() {
		for (let i = 0; i < saved.length; i++) {
			const node = saved[i];
			const child = document.getElementById('code').children[i];

			for (const subchild of child.children[0].children) {
				if (subchild.getAttribute('is')) {
					if (subchild.tagName == 'DIV') subchild.innerHTML = node[1][subchild.getAttribute('is')];
					else subchild.value = node[1][subchild.getAttribute('is')];
				}
			}
		}
	}

	function updateState() {
		currentState[currentNode] = [];

		for (const child of document.getElementById('code').children) {
			let node = '';
			let attributes = {};
			
			for (const subchild of child.children[0].children) {
				if (subchild.tagName == 'TYPE') node = subchild.innerHTML;
				else attributes[subchild.getAttribute('is')] = subchild.tagName == 'DIV' ? subchild.innerHTML : subchild.value;
			}

			currentState[currentNode].push([node, attributes]);
		}
	}

	// It doesn't work without setTimeout and I don't know why
	setTimeout(() => {
		load();
	}, 1);
	setTimeout(() => {
		hydrate();
	}, 1);

	const addItem = (item) => { list = [...list, {id: list.length + 1, component: item}]; updateState() }

	document.addEventListener('keydown', (e) => {
		// If 'n' key is pressed
		if (e.altKey == true && e.key == 'n') 
			hidden = false;
		else if (e.key == 'Escape')
			hidden = true;
	})
</script>

<main >
	<nav>
		<Breadcrumb items={['Projects', 'myProject', 'main']}/>
		<button on:click="{compile}">Compile</button>
	</nav>
	<div id="container">
		<List 
			list={list}
			key="id"
			onDrop={updateState}
			on:sort={sortList}
		/>
	</div>
	<div id="nodes-modal" class:hidden={hidden} on:click|self="{() => {hidden = true}}">
		<Nodes add={addItem} searchBar/>
	</div>
</main>

<style>
	nav {
		border-bottom: 1px solid #343434;
		width: 100vw;
		height: 60px;
		
		display: flex;
		flex-direction: row;
		gap: 12px;
		padding: 12px;
		
		align-items: center;
		justify-content: flex-start;
	}

	button {
		padding: 10px;
		height: 36px;
		width: fit-content;

		background: #171717;
		border: 1px solid #343434;
		border-radius: 4px;

		color: #5C6DFF;
		cursor: pointer;

		margin-left: auto;
	}

	button:hover {
		border: 1px solid #565656;
	}

	#container {
		width: 100vw;
		height: 100%;

		padding: 12px;
	}

	#nodes-modal {
		position: fixed;
		z-index: 1;
		left: 0;
		top: 0;
		width: 100vw;
		height: 100vh;
		overflow: auto;
		background-color: rgba(0, 0, 0, 0.4);
		display: flex;
		padding-top: 15%;
		justify-content: center;
	}

	.hidden {
		display: none !important;
	}
</style>
