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
  import DataEditor from './Editors/DataEditor.svelte'
  import DataViewports from './DataDisplays/DataViewports.svelte'
  import DisplayHeader from './DataDisplays/Header/DisplayHeader.svelte'
</script>

<main class="dataEditor">
  <div class="data-viewports">
    <DisplayHeader on:clearDataDisplays />
    <DataViewports
      on:clearDataDisplays
      on:scrolledToTop
      on:scrolledToEnd
      on:applyChanges
      on:handleEditorEvent
      on:scrollBoundary
      on:traverse-file
      on:seek
    />
  </div>
  <div class="data-editor">
    <DataEditor on:applyChanges on:handleEditorEvent on:clearDataDisplays />
  </div>
</main>

<style lang="scss">
  .data-viewports {
    display: flex;
    flex-direction: column;
  }
  .data-editor {
    display: flex;
    flex-direction: column;
    width: 100%;
  }
</style>
