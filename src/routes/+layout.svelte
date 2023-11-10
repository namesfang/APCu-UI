<script lang="ts">
  import '@/app.scss';

  export let data

  let connections = [
    {
      id: 1,
      label: '默认连接',
    },
    {
      id: 2,
      label: '大屏设备',
    },
    {
      id: 3,
      label: '新服务器',
    },
  ]

  let westStyle = ''
  let mousedowned = false

  const mousedown = function() {
    mousedowned = true;
  }

  const mousemove = function({clientX}: MouseEvent) {
    if(!mousedowned) {
      return
    }
    
    if(clientX < 320 || clientX > 640) {
      return
    }
    westStyle = `width: ${clientX}px`
  }

  const mouseup = function() {
    mousedowned = false
  }

</script>

<svelte:window on:mousemove={mousemove} on:mouseup={mouseup}></svelte:window>

<div class="layout-header">
  <div class="brand">APCu WebUI</div>
  <div class="connections">
    <ul>
      <li>
        <button>
          <Icon type="add"></Icon>
          <span>添加连接</span>
        </button>
      </li>
      {#each connections as t}
        <li>
          <a href="/connections?id={t.id}" class={data.id == t.id ? 'active' : ''}>{t.label}</a>
        </li>
      {/each}
    </ul>
  </div>
</div>
<div class="layout-container">
  <div class="layout-west" style={westStyle}>
    <ul>
      <li>
        <a href="/">首页</a>
      </li>
      <li>
        <a href="/content">文章</a>
      </li>
      <li>
        <a href="/about">关于</a>
      </li>
    </ul>
    <button on:mousedown={mousedown} class="resizeable"></button>
  </div>
  <div class="layout-main">
      <slot></slot>
  </div>
</div>

<style lang="scss">
  .layout-header {
    height: 46px;
    display: flex;
    // position: relative;
    // &::after {
    //   content: "";
    //   border-bottom: 1px solid #e1e6e6;
    //   position: absolute;
    //   bottom: 0;
    //   left: 0;
    //   right: 0;
    //   transform: scaleY(.5);
    // }
    .brand {
      width: 320px;
      display: flex;
      align-items: center;
      justify-content: center;
      font: {
        size: 18px;
        weight: bold;
      }
    }

    .connections {
      flex: 1;
      ul {
        display: flex;
        align-items: center;
        align-content: center;
        li {
          margin-left: 20px;
          a {
            height: 46px;
            line-height: 46px;
            font-size: 14px;
            text-decoration: none;
            color: #363636;
          }
          .active {
            color: #39f;
          }
        }
      }
    }
  }

  .layout-container {
    flex: 1;
    display: flex;

    .layout-west {
      width: 320px;
      position: relative;
      .resizeable {
        width: 10px;
        height: 100%;
        position: absolute;
        right: -5px;
        top: 0;
        z-index: 1;
        cursor: e-resize;
        background-color: transparent;
        border: 0;
        padding: 0;
      }
    }
    .layout-main {
      flex: 1;
      padding: 20px;
      background-color: #fff;
      border-radius: 10px 0 0 0;
      position: relative;

      $border: 1px solid #e1e1e1;

      &::before {
        content: "";
        width: 200%;
        height: 200%;
        z-index: -1;
        border: {
          top: $border;
          left: $border;
        }
        box-sizing: border-box;
        position: absolute;
        top: 0;
        left: 0;
        border-radius: 20px;
        transform: scale(.5);
        transform-origin: 0 0;
      }
    }
  }
</style>