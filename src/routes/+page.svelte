<script lang="ts">
    import type { TreeNodeInfo } from "./TreeNodeInfo";
    import Tree from "./Tree.svelte";
    import { writable } from "svelte/store";

    const treeStructure: TreeNodeInfo[] = [{
      isRoot: true,
      name: "nodeA",
      childNodes: [
        { isRoot: false, name: "nodeB", childNodes: [] },
        { isRoot: false, name: "nodeC", childNodes: [] },
      ],
    }]

    const state = writable(treeStructure);

    let newChildNodeName: string;
    let parentNodeName: string;

    const addNodeBtnA_OnClick = () => {
        const findNodeByName = (name: string, nodes: TreeNodeInfo[]): TreeNodeInfo | undefined => {          
            for (const node of nodes) {
                if (node.name === name) return node;            
                const foundNode = findNodeByName(name, node.childNodes);
                if (foundNode) return foundNode;
            }  
            return undefined;
        }
        // "state" is the store that's passed to the the Tree component
        state.update(curr => {         
            findNodeByName(parentNodeName, curr)?.childNodes.push({ isRoot: false, name: newChildNodeName, childNodes: []});
            return curr;
        })

        console.log($state); 
    }

    // Calling this function also messes up the store, because after the call the second method (addNodeBtnB_OnClick) also stops working
    const addNodeBtnB_OnClick = () => {
        const addNode = (parentNodeName: string,
            currentNode: TreeNodeInfo,
            nodeToAdd: TreeNodeInfo) : TreeNodeInfo => {
            return currentNode.name === parentNodeName
            ? { ...currentNode, childNodes: [...currentNode.childNodes, nodeToAdd] }
            : {
                ...currentNode,
                childNodes: currentNode.childNodes.map((node) => addNode(parentNodeName, node, nodeToAdd))
                }
        };

        state.update(
          curr => curr.map((node => addNode(parentNodeName, node, { isRoot: false, name: newChildNodeName, childNodes: []}))
        ));

        console.log($state); // The store is updated, but reactivity is not triggered at all
    }


  </script>
  
  <main>
    <div>
      <span>NEW CHILDNODE NAME:</span>
      <input bind:value={newChildNodeName}>
    </div>
    <div>
      <span>PARENT NODE NAME:</span>
      <input bind:value={parentNodeName}>
    </div>
    <button on:click={addNodeBtnA_OnClick}>Add Node (kinda working)</button> <!-- This is not working at all -->
    <button on:click={addNodeBtnB_OnClick}>Add Node (not working)</button> <!-- The node is added, but it's visible only after reopenning the node -->
    <Tree state={state} />
  </main>

  <slot/>

  <style>
    main {
      display: flex;
      flex-direction: column;
      width: 25%;
    }

    button {
      width: 25%;
    }
  </style>