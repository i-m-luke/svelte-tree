<script lang="ts">
    import type { TreeNodeInfo } from "./TreeNodeInfo";

    export let nodeInfo: TreeNodeInfo;
 
    let isOpen: boolean = true;
    let childNodes: TreeNodeInfo[] = nodeInfo.childNodes;
    $: nodeState = isOpen ? "(opened)" : "(closed)";
 
    const toggleIsOpen: () => void = () => (isOpen = !isOpen);
 </script>
 
 <div class="node-container">
    <div class="parent-node">
       {#if !nodeInfo.isRoot}
          <div class="connection-container">
             <div class="connection" />
          </div>
       {/if}
          
        <button on:click={toggleIsOpen}>
            {`${nodeInfo.name} ${nodeState}`}
        </button>
    </div>
 
    {#if isOpen}
       <div class="child-nodes">
          {#each childNodes as childNode}
             <svelte:self nodeInfo={childNode} />
          {/each}
       </div>
    {/if}
 </div>
 
 <style> 
    .node-container,
    .child-nodes {
       display: flex;
       flex-direction: column;
    }
 
    .child-nodes {
       margin-left: 24px;
    }
 
    .parent-node {
       display: flex;
       flex-direction: row;
    }
 
    .connection-container {
       width: 2%;
    }
 
    .connection {
       position: relative;
       right: 0px;
       width: 50%;
       height: 50%;
       border: 1px solid black;
       border-width: 0px 0px 1px 1px;
    }
 </style>
 