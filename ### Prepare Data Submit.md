### Prepare Data Submit
Setelah validasi form dilakukan, langkah berikutnya adalah mempersiapkan data sebelum dikirimkan ke
server. Kita bisa menggunakan object FormData
(https://developer.mozilla.org/en-US/docs/Web/API/FormData/FormData)  untuk mem-packing data hasil isian form menjadi sebuah object.

```javascript
let formData = new FormData();
// tambahkan satu persatu field
formData.append('username', 'Chris');
```

Berikut ini implementasi pada kasus kita.
```javascript
if( this.errors.length === 0 ){
    alert('Terima kasih telah mengisi data dengan benar!')
    // persiapkan data
    let formData = new FormData()
    formData.append('title', this.title)
    formData.append('description', this.description)
    formData.append('authors', this.authors)
    formData.append('price', this.price)
    formData.append('categories', this.categories)
    // kirim data ke server
}
```

Di samping cara di atas yaitu manual satu persatu dalam memasukkan data field, FormData juga
menyediakan cara cepat untuk memasukkan semua data field yang dimiliki form ke dalam objek formData.
Berikut yang kita dapat dari dokumentasi FormData.

```javascript
let myForm = document.getElementById('myForm');
formData = new FormData(myForm);
```
Dengan sedikit modifikasi maka implementasi pada kasus kita menjadi sebagai berikut

```javascript
// persiapkan data
let formBook = this.$refs.formBook
formData = new FormData(formBook);
// kirim data ke server
```
selesai

> Catatan: untuk bisa menggunakan cara singkat ini syaratnya satu yaitu field pada form harus ada atribut name-nya.

### Send Data To Server
Setelah data di-bundle dalam satu objek formData, maka data siap untuk dikirim ke server. Pada bagian ini,
kita akan mensimulasikan pengiriman data form ke server menggunakan PHP native.

> Catatan: 80 adalah nomer port dari webserver, kita bebas mengubahnya dengan nomer lain yang sedang tidak digunakan.

## Component
### Intro
Pada bab ini kita akan belajar tentang konsep component pada Vue dan penggunaannya pada aplikasi.
Konsep component pada Vue mirip dengan konsep Web Components.

### Component Dasar 
Component merupakan sub class/objek dari Vue yang bisa kita buat untuk berbagai tujuan misalnya
memecah kompleksitas kode, reusabilitas kode, dan modularitas kode. Setiap component memiliki lifecycle
yang sama dengan objek Vuenya.

Untuk membuat sebuah component baru, cukup dengan
> Vue.component('nama-component', { /* options */ }).

```javascript
Vue.component('hello-world', {
data () {
    return {
        message: 'Hello world!'
    }
},
    template: '<h1> {{ message }}</h1>'
})
var vm = new Vue({
    el: '#app',
})
```

### Penamaan Component
Terdapat dua format penamaan component pada Vue
1. kebab-case
Format kebab-case artinya menggunakan huruf lowercase dan antar kata dipisah dengan tanda - (dash).

```javascript
Vue.component('my-component-name', {/* ... */})
```

Ketika kita mendefinisikan component dengan menggunakan format kebab-case, maka komponen tersebut
dapat diakses menggunakan custom element <my-component-name>.


2. PascalCase
Format PascalCase artinya menggunakan huruf capital pada huruf pertama setiap kata di mana antar kata
tidak dipisah.

```javascript
Vue.component('MyComponentName', { /* ... */ })
```
Ketika kita mendefinisikan component dengan menggunakan format PascalCase, maka kita bisa memilih
untuk mengakses komponen tersebut dengan cara <my-component-name> atau <MyComponentName>.
Namun hanya elemen kebab-case yang dianggap valid oleh DOM.

### Component Registration
Terdapat dua cara untuk meregister component pada Vue supaya bisa digunakan yaitu global dan local.

### Global Component

Register component secara global akan membuat component tersebut bisa digunakan oleh semua objek
utama (root) Vue, cara meregister component secara global sebagaimana contoh di atas yaitu menggunakan
method

```javascript
 Vue.component('component-a', {
            template: '<p>Komponen a</p>'
        })

        // component b
        Vue.component('component-b', {
            template: '<p>komponen b </p>'
        })

        // component c
        Vue.component('component-c', {
            template: '<p>komponen c</p>'
        })

        new Vue({
            el: '#app1'
        })

        new Vue({
            el: '#app2'
        })
```

Implementasi pada template
```html
<div id="app1">
        <hello-world></hello-world>
        <component-a></component-a>
        <component-b></component-b>
        <component-c></component-c>
    </div>

    <div id="app2">
        <component-a></component-a>
    </div>
```

### Local Component
Register component secara local artinya component tersebut diregister pada suatu objek Vue dan hanya bisa
digunakan pada objek tersebut saja.
Untuk mendefinisikan component local cukup dengan menggunakan object Javascript biasa

```javascript
 var ComponentLocalA = {
            template: '<p>Component local A</p>'
        }

        var ComponentLocalB = {
            template: '<p>Component local B</p>'
        }

        var ComponentLocalC = {
            template: '<p>Component local C</p>'
        }
```

Adapun untuk meregister pada objek vue menggunakan properti components.

```javascript
 new Vue({
            el: '#app3',
            components: {
                'component-local-a': ComponentLocalA,
                'component-local-b': ComponentLocalB,
                'component-local-c': ComponentLocalC
            }
        })
```

Pada template
```html
    <div id="app3">
        <component-local-a> </component-local-a>
        <component-local-b></component-local-b>
        <component-local-c></component-local-c>
    </div>
```

Sebaliknya jika kita register suatu component ke objek Vue pertama saja sedangkan objek Vue kedua tidak.

### Deklarasi Properti Data
Strukturnya components secara umum hampir sama dengan objek utama Vue, salah satu yang membedakan
adalah definisi properti data pada component tidak lagi sebagai object melainkan sebagai sebuah fungsi.


```javascript
data: {
message: 'Hello world'
}
// menjadi
data () {
    return {
    message: 'Hello world!'
}
}
```

### Reusable Component
Component ini kemudian dapat digunakan berkali-kali atau reusable.

### Component Lanjutan
### Kirim Data ke Component
Vue mempunyai mekanisme untuk mengirimkan atau mengeset suatu data pada component yaitu dengan
menggunakan properti props
Untuk mendefinisikan props, pada component caranya cukup dengan menambahkan properti props.

```javascript
props: [ 'nama_props1', 'nama_props2'],
```

```javascript
template : `<div> {{ nama_props1 }} </div>`
```

