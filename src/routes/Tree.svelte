<script lang="ts">
  import { getContext } from "svelte";
    import  TreeNode from "./TreeNode.svelte";
    import type { TreeNodeInfo } from "./TreeNodeInfo";
    import type { Writable} from "svelte/store";
 
    export let state: Writable<TreeNodeInfo[]>;
 </script>
  
 {#each $state as node}
    <TreeNode nodeInfo={node} />
 {/each}
 
 