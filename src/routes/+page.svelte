<script lang="ts">
    import Tree from "./Tree.svelte";
    import { writable } from "svelte/store";
  
    const treeNodes = [
      { isRoot: true, name: "nodeA", childNodes: [
        { isRoot: false, name: "nodeB", childNodes: []},
        { isRoot: false, name: "nodeC", childNodes: []},
      ]}
    ]
    const treeState = writable(treeNodes);

    const addNode = () => treeState.update(curr => {
        curr[0].childNodes.push({isRoot: false, name: "NEW NODE", childNodes: []});
        return curr;
    })

    const addNodeBtnOnClick = () => {
        addNode();
        console.log($treeState); 
    }
  </script>

  
  <main>

    <button on:click={addNodeBtnOnClick}>ADD NODE</button>
    <Tree state={treeState}/>
  </main>