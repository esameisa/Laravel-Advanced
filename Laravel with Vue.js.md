# Laravel with Vue.js:

## Vue.nextTick( [callback, context] )

Defer the callback to be executed after the next DOM update cycle. Use it immediately after you’ve changed some data to wait for the DOM update.

Example:

HTML:
```
<div class="alert alert-danger" v-if="Object.keys(requestErrors).length !== 0">
    <ul>
        <li v-for="error in requestErrors">
            {{ error }}
        </li>
    </ul>
</div>
```
Vue.js:
```
scrollToError() {
    this.$nextTick(() => {
        let domRect = document.querySelector('.alert-danger')?.getBoundingClientRect();
        if (domRect) {
            window.scrollTo(
                domRect.left + document.documentElement.scrollLeft,
                domRect.top + document.documentElement.scrollTop - 10
            );
        }
    });
}
```

## Transfer: 

Library to handel permissions for example, see documentation here: https://www.antdv.com/components/transfer/

```
components: {
    'el-transfer': () => import('element-ui/lib/transfer')
},

// CSS
<style scoped lang="scss">
    @import "~element-ui/lib/theme-chalk/transfer.css";
</style>

// Element in html:
<el-transfer
    style="text-align: left; display: inline-block; width: 100%"
    v-model="agent.permissions"
    filterable
    filter-placeholder="search"
    :titles="['Permissions', 'Assigned']"
    :button-texts="['Remove', 'Assign']"
    :data="permissions"
></el-transfer>
```

##Replace then split element then join
```
render: (data) => {
    let permissionsStr = ``;

    data.map(p => {
        permissionsStr += `<li>${p.name.replace('merchant-', '').split('-').join(' ')}</li>`
    })

    return `<ol style="list-style-type: symbols('-'); text-align: left; margin-left: 15px">${permissionsStr}</ol>`;
}
```