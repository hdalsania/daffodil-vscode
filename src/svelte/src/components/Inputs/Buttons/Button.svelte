<!--
Licensed to the Apache Software Foundation (ASF) under one or more
contributor license agreements.  See the NOTICE file distributed with
this work for additional information regarding copyright ownership.
The ASF licenses this file to You under the Apache License, Version 2.0
(the "License"); you may not use this file except in compliance with
the License.  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->
<script lang="ts">
  import Tooltip from '../../layouts/Tooltip.svelte'
  import FlexContainer from '../../layouts/FlexContainer.svelte'
  import { onMount } from 'svelte'
  import { UIThemeCSSClass } from '../../../utilities/colorScheme'
  import { tooltipsEnabled } from '../../../stores'
  import { shouldCollapseContent } from '../../../utilities/display'
  export let fn: (event?: Event) => void
  export let disabledBy = false
  export let width = ''
  export let fixedWidth = ''

  onMount(() => {
    collapseContent = shouldCollapseContent()
  })

  export let description = ''
  export let tooltipAlwaysEnabled = false

  let collapseContentFn: NodeJS.Timeout
  let collapseContent = false

  $: $tooltipsEnabled = collapseContent

  window.addEventListener('resize', () => {
    clearTimeout(collapseContentFn)
    collapseContentFn = setTimeout(() => {
      collapseContent = shouldCollapseContent()
    }, 100)
  })
</script>

<Tooltip alwaysEnabled={tooltipAlwaysEnabled} {description}>
  {#if collapseContent}
    <button
      class={$UIThemeCSSClass + ' collapsed'}
      disabled={disabledBy}
      on:click={!disabledBy ? fn : () => {}}
      style:width={fixedWidth}
    >
      <FlexContainer
        --dir="row"
        --align-items="center"
        --justify-content="center"
      >
        <svelte:fragment>
          <slot name="left" />
          <slot name="right" />
        </svelte:fragment>
      </FlexContainer>
    </button>
  {:else}
    <button
      class={$UIThemeCSSClass}
      disabled={disabledBy}
      on:click={!disabledBy ? fn : () => {}}
      style:width={fixedWidth.length > 0 ? fixedWidth : width}
    >
      <FlexContainer
        --dir="row"
        --align-items="center"
        --justify-content="center"
      >
        <svelte:fragment>
          <slot name="left" />
          <slot />
          <slot name="right" />
        </svelte:fragment>
      </FlexContainer>
    </button>
  {/if}
</Tooltip>

<style lang="scss">
  button {
    width: var(--width, 80pt);
    outline: none;
  }
  button.collapsed {
    width: 25pt;
    margin-left: 5px;
    margin-right: 5px;
  }
</style>
