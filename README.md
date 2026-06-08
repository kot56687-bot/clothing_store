
<h2 class="sr-only">Информационная система магазина одежды — управление данными</h2>

<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: var(--font-sans); }

.app { display: flex; height: 100vh; min-height: 600px; border: 0.5px solid var(--color-border-tertiary); border-radius: var(--border-radius-lg); overflow: hidden; }

.sidebar {
  width: 200px; min-width: 200px;
  background: var(--color-background-secondary);
  border-right: 0.5px solid var(--color-border-tertiary);
  display: flex; flex-direction: column;
  padding: 16px 0;
}
.sidebar-brand {
  padding: 0 16px 16px;
  border-bottom: 0.5px solid var(--color-border-tertiary);
  margin-bottom: 12px;
}
.sidebar-brand .logo { font-size: 15px; font-weight: 500; color: var(--color-text-primary); }
.sidebar-brand .sub { font-size: 11px; color: var(--color-text-secondary); margin-top: 2px; }
.nav-group { padding: 0 8px; margin-bottom: 4px; }
.nav-label { font-size: 10px; font-weight: 500; color: var(--color-text-tertiary); letter-spacing: 0.08em; text-transform: uppercase; padding: 4px 8px 4px; }
.nav-item {
  display: flex; align-items: center; gap: 8px;
  padding: 7px 10px; border-radius: var(--border-radius-md);
  font-size: 13px; color: var(--color-text-secondary);
  cursor: pointer; transition: background 0.12s;
}
.nav-item:hover { background: var(--color-background-primary); color: var(--color-text-primary); }
.nav-item.active { background: var(--color-background-primary); color: var(--color-text-primary); font-weight: 500; }
.nav-item i { font-size: 15px; }

.main { flex: 1; display: flex; flex-direction: column; overflow: hidden; }

.topbar {
  display: flex; align-items: center; justify-content: space-between;
  padding: 12px 20px;
  border-bottom: 0.5px solid var(--color-border-tertiary);
  background: var(--color-background-primary);
}
.page-title { font-size: 16px; font-weight: 500; color: var(--color-text-primary); }
.topbar-actions { display: flex; gap: 8px; align-items: center; }
.search-wrap { position: relative; }
.search-wrap i { position: absolute; left: 9px; top: 50%; transform: translateY(-50%); color: var(--color-text-tertiary); font-size: 14px; pointer-events: none; }
.search-wrap input { padding: 6px 10px 6px 30px; font-size: 13px; width: 200px; border-radius: var(--border-radius-md); }
.btn-add {
  display: flex; align-items: center; gap: 5px;
  padding: 7px 14px; font-size: 13px; font-weight: 500;
  background: var(--color-text-primary); color: var(--color-background-primary);
  border: none; border-radius: var(--border-radius-md); cursor: pointer;
}
.btn-add:hover { opacity: 0.85; }
.btn-add i { font-size: 14px; }

.content { flex: 1; overflow-y: auto; padding: 20px; }

.stats-row { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; margin-bottom: 20px; }
.stat-card {
  background: var(--color-background-secondary);
  border-radius: var(--border-radius-md);
  padding: 14px 16px;
}
.stat-label { font-size: 12px; color: var(--color-text-secondary); margin-bottom: 6px; display: flex; align-items: center; gap: 5px; }
.stat-value { font-size: 22px; font-weight: 500; color: var(--color-text-primary); }
.stat-sub { font-size: 11px; color: var(--color-text-tertiary); margin-top: 3px; }

.table-card {
  background: var(--color-background-primary);
  border: 0.5px solid var(--color-border-tertiary);
  border-radius: var(--border-radius-lg);
  overflow: hidden;
}
table { width: 100%; border-collapse: collapse; font-size: 13px; }
thead tr { background: var(--color-background-secondary); }
th { padding: 10px 14px; text-align: left; font-weight: 500; font-size: 12px; color: var(--color-text-secondary); border-bottom: 0.5px solid var(--color-border-tertiary); }
td { padding: 10px 14px; border-bottom: 0.5px solid var(--color-border-tertiary); color: var(--color-text-primary); }
tr:last-child td { border-bottom: none; }
tr:hover td { background: var(--color-background-secondary); }
.td-actions { display: flex; gap: 4px; }
.btn-icon {
  width: 28px; height: 28px; border-radius: var(--border-radius-md);
  display: flex; align-items: center; justify-content: center;
  cursor: pointer; background: transparent; border: 0.5px solid transparent;
  color: var(--color-text-secondary); font-size: 14px; transition: all 0.1s;
}
.btn-icon:hover { background: var(--color-background-secondary); border-color: var(--color-border-secondary); color: var(--color-text-primary); }
.btn-icon.danger:hover { background: var(--color-background-danger); border-color: var(--color-border-danger); color: var(--color-text-danger); }

.badge {
  display: inline-block; padding: 2px 8px; border-radius: 20px; font-size: 11px; font-weight: 500;
}
.badge-success { background: var(--color-background-success); color: var(--color-text-success); }
.badge-warning { background: var(--color-background-warning); color: var(--color-text-warning); }
.badge-info { background: var(--color-background-info); color: var(--color-text-info); }
.badge-danger { background: var(--color-background-danger); color: var(--color-text-danger); }

.overlay {
  display: none; position: fixed; inset: 0;
  background: rgba(0,0,0,0.35); z-index: 100;
  align-items: center; justify-content: center;
}
.overlay.open { display: flex; }
.modal {
  background: var(--color-background-primary);
  border: 0.5px solid var(--color-border-tertiary);
  border-radius: var(--border-radius-lg);
  width: 420px; max-width: 95vw;
  padding: 24px;
}
.modal-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; }
.modal-title { font-size: 15px; font-weight: 500; }
.modal-close { background: none; border: none; cursor: pointer; color: var(--color-text-secondary); font-size: 18px; display: flex; }
.form-group { margin-bottom: 14px; }
label { display: block; font-size: 12px; font-weight: 500; color: var(--color-text-secondary); margin-bottom: 5px; }
input[type=text], input[type=email], input[type=number], input[type=date], select, textarea {
  width: 100%; font-size: 13px;
}
textarea { height: 70px; resize: vertical; }
.form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.modal-footer { display: flex; justify-content: flex-end; gap: 8px; margin-top: 20px; }
.btn-cancel { padding: 8px 16px; font-size: 13px; border-radius: var(--border-radius-md); cursor: pointer; }
.btn-submit { padding: 8px 18px; font-size: 13px; font-weight: 500; border-radius: var(--border-radius-md); cursor: pointer; background: var(--color-text-primary); color: var(--color-background-primary); border: none; }
.btn-submit:hover { opacity: 0.85; }

.confirm-modal { background: var(--color-background-primary); border: 0.5px solid var(--color-border-tertiary); border-radius: var(--border-radius-lg); width: 340px; padding: 24px; }
.confirm-icon { width: 44px; height: 44px; border-radius: 50%; background: var(--color-background-danger); display: flex; align-items: center; justify-content: center; margin-bottom: 12px; }
.confirm-icon i { font-size: 20px; color: var(--color-text-danger); }
.confirm-text { font-size: 13px; color: var(--color-text-secondary); margin-top: 6px; }
.empty-state { text-align: center; padding: 40px; color: var(--color-text-secondary); font-size: 13px; }
.empty-state i { font-size: 32px; display: block; margin-bottom: 8px; color: var(--color-text-tertiary); }
</style>

<div class="app">
  <div class="sidebar">
    <div class="sidebar-brand">
      <div class="logo"><i class="ti ti-hanger" aria-hidden="true"></i> StyleStore</div>
      <div class="sub">Управление магазином</div>
    </div>
    <div class="nav-group">
      <div class="nav-label">Каталог</div>
      <div class="nav-item active" data-section="products" onclick="switchSection('products')"><i class="ti ti-shirt" aria-hidden="true"></i> Товары</div>
      <div class="nav-item" data-section="categories" onclick="switchSection('categories')"><i class="ti ti-tag" aria-hidden="true"></i> Категории</div>
      <div class="nav-item" data-section="brands" onclick="switchSection('brands')"><i class="ti ti-award" aria-hidden="true"></i> Бренды</div>
    </div>
    <div class="nav-group">
      <div class="nav-label">Продажи</div>
      <div class="nav-item" data-section="customers" onclick="switchSection('customers')"><i class="ti ti-users" aria-hidden="true"></i> Клиенты</div>
      <div class="nav-item" data-section="orders" onclick="switchSection('orders')"><i class="ti ti-shopping-cart" aria-hidden="true"></i> Заказы</div>
    </div>
    <div class="nav-group">
      <div class="nav-label">Склад</div>
      <div class="nav-item" data-section="stock" onclick="switchSection('stock')"><i class="ti ti-building-warehouse" aria-hidden="true"></i> Остатки</div>
      <div class="nav-item" data-section="suppliers" onclick="switchSection('suppliers')"><i class="ti ti-truck" aria-hidden="true"></i> Поставщики</div>
    </div>
    <div class="nav-group">
      <div class="nav-label">Персонал</div>
      <div class="nav-item" data-section="employees" onclick="switchSection('employees')"><i class="ti ti-id-badge" aria-hidden="true"></i> Сотрудники</div>
    </div>
  </div>

  <div class="main">
    <div class="topbar">
      <div class="page-title" id="page-title">Товары</div>
      <div class="topbar-actions">
        <div class="search-wrap">
          <i class="ti ti-search" aria-hidden="true"></i>
          <input type="text" id="search-input" placeholder="Поиск..." oninput="filterTable()" />
        </div>
        <button class="btn-add" onclick="openAddModal()"><i class="ti ti-plus" aria-hidden="true"></i> Добавить</button>
      </div>
    </div>
    <div class="content">
      <div class="stats-row" id="stats-row"></div>
      <div class="table-card">
        <div id="table-container"></div>
      </div>
    </div>
  </div>
</div>

<div class="overlay" id="form-overlay">
  <div class="modal">
    <div class="modal-header">
      <div class="modal-title" id="modal-title">Добавить</div>
      <button class="modal-close" onclick="closeModal()"><i class="ti ti-x"></i></button>
    </div>
    <div id="modal-body"></div>
    <div class="modal-footer">
      <button class="btn-cancel" onclick="closeModal()">Отмена</button>
      <button class="btn-submit" onclick="submitForm()">Сохранить</button>
    </div>
  </div>
</div>

<div class="overlay" id="confirm-overlay">
  <div class="confirm-modal">
    <div class="confirm-icon"><i class="ti ti-trash"></i></div>
    <div style="font-size:15px; font-weight:500;" id="confirm-title">Удалить запись?</div>
    <div class="confirm-text" id="confirm-text">Это действие нельзя отменить.</div>
    <div class="modal-footer">
      <button class="btn-cancel" onclick="closeConfirm()">Отмена</button>
      <button class="btn-submit" style="background:var(--color-text-danger);" onclick="confirmDelete()">Удалить</button>
    </div>
  </div>
</div>

<script>
const data = {
  products: [
    { id:1, name:'Футболка Nike', category:'Футболки', brand:'Nike', size:'M', color:'Белый', season:'лето', price:1990, status:'в наличии' },
    { id:2, name:'Джинсы Levis 501', category:'Джинсы', brand:'Levis', size:'32', color:'Синий', season:'демисезон', price:5990, status:'в наличии' },
    { id:3, name:'Куртка Adidas', category:'Куртки', brand:'Adidas', size:'L', color:'Чёрный', season:'зима', price:8990, status:'в наличии' },
    { id:4, name:'Кроссовки Zara', category:'Обувь', brand:'Zara', size:'42', color:'Белый', season:'лето', price:4490, status:'нет в наличии' },
    { id:5, name:'Свитер Nike', category:'Футболки', brand:'Nike', size:'XL', color:'Серый', season:'зима', price:3290, status:'в наличии' },
  ],
  categories: [
    { id:1, name:'Футболки', description:'Повседневная верхняя одежда' },
    { id:2, name:'Джинсы', description:'Джинсовые брюки' },
    { id:3, name:'Куртки', description:'Верхняя одежда' },
    { id:4, name:'Обувь', description:'Мужская и женская обувь' },
  ],
  brands: [
    { id:1, name:'Nike', country:'США', description:'Спортивная одежда и обувь' },
    { id:2, name:'Adidas', country:'Германия', description:'Мировой спортивный бренд' },
    { id:3, name:'Zara', country:'Испания', description:'Модная повседневная одежда' },
    { id:4, name:'Levis', country:'США', description:'Классические джинсы' },
  ],
  customers: [
    { id:1, first_name:'Анна', last_name:'Смирнова', phone:'+79001112233', email:'anna@mail.ru', reg_date:'2024-01-15', status:'постоянный' },
    { id:2, first_name:'Иван', last_name:'Кузнецов', phone:'+79002223344', email:'ivan@mail.ru', reg_date:'2024-02-10', status:'обычный' },
    { id:3, first_name:'Мария', last_name:'Иванова', phone:'+79003334455', email:'maria@mail.ru', reg_date:'2024-03-05', status:'постоянный' },
  ],
  orders: [
    { id:1, customer:'Анна Смирнова', order_date:'2024-04-10', status:'оплачен', total:7980 },
    { id:2, customer:'Иван Кузнецов', order_date:'2024-04-12', status:'новый', total:5990 },
    { id:3, customer:'Мария Иванова', order_date:'2024-04-14', status:'доставлен', total:13480 },
  ],
  stock: [
    { id:1, product:'Футболка Nike', warehouse:'Основной склад', quantity:45, last_update:'2024-04-01' },
    { id:2, product:'Джинсы Levis 501', warehouse:'Основной склад', quantity:18, last_update:'2024-04-01' },
    { id:3, product:'Куртка Adidas', warehouse:'Региональный склад', quantity:9, last_update:'2024-04-02' },
    { id:4, product:'Кроссовки Zara', warehouse:'Основной склад', quantity:0, last_update:'2024-04-03' },
  ],
  suppliers: [
    { id:1, name:'ООО СпортПоставка', contact:'Сидоров А.А.', phone:'+79004445566', email:'sport@supply.ru', address:'г. Москва' },
    { id:2, name:'Fashion Group', contact:'Крылова Е.В.', phone:'+79005556677', email:'fashion@supply.ru', address:'г. Санкт-Петербург' },
  ],
  employees: [
    { id:1, first_name:'Дмитрий', last_name:'Соколов', position:'Продавец', phone:'+79010001122', hire_date:'2023-01-10' },
    { id:2, first_name:'Елена', last_name:'Морозова', position:'Менеджер', phone:'+79020002233', hire_date:'2022-06-15' },
    { id:3, first_name:'Алексей', last_name:'Волков', position:'Кладовщик', phone:'+79030003344', hire_date:'2023-03-20' },
  ],
};

const config = {
  products: {
    title: 'Товары',
    cols: ['id','name','category','brand','size','color','season','price','status'],
    heads: ['#','Название','Категория','Бренд','Размер','Цвет','Сезон','Цена','Статус'],
    badge: 'status',
    badgeMap: { 'в наличии':'success', 'нет в наличии':'danger', 'архив':'warning' },
    price: 'price',
    stats: () => {
      const d = data.products;
      const instock = d.filter(x=>x.status==='в наличии').length;
      const total = d.reduce((s,x)=>s+x.price,0);
      return [
        {label:'Всего товаров', icon:'ti-shirt', value:d.length},
        {label:'В наличии', icon:'ti-check', value:instock},
        {label:'Средняя цена', icon:'ti-coin', value:Math.round(total/d.length)+'₽'},
        {label:'Категорий', icon:'ti-tag', value:data.categories.length},
      ];
    },
    form: () => `
      <div class="form-group"><label>Название</label><input type="text" id="f-name" placeholder="Название товара"></div>
      <div class="form-row">
        <div class="form-group"><label>Категория</label><select id="f-category">${data.categories.map(c=>`<option>${c.name}</option>`).join('')}</select></div>
        <div class="form-group"><label>Бренд</label><select id="f-brand">${data.brands.map(b=>`<option>${b.name}</option>`).join('')}</select></div>
      </div>
      <div class="form-row">
        <div class="form-group"><label>Размер</label><input type="text" id="f-size" placeholder="M, L, 42..."></div>
        <div class="form-group"><label>Цвет</label><input type="text" id="f-color" placeholder="Цвет"></div>
      </div>
      <div class="form-row">
        <div class="form-group"><label>Сезон</label><select id="f-season"><option>лето</option><option>зима</option><option>демисезон</option></select></div>
        <div class="form-group"><label>Цена (₽)</label><input type="number" id="f-price" placeholder="0"></div>
      </div>
      <div class="form-group"><label>Статус</label><select id="f-status"><option>в наличии</option><option>нет в наличии</option><option>архив</option></select></div>`,
    getValues: () => ({
      name: document.getElementById('f-name').value,
      category: document.getElementById('f-category').value,
      brand: document.getElementById('f-brand').value,
      size: document.getElementById('f-size').value,
      color: document.getElementById('f-color').value,
      season: document.getElementById('f-season').value,
      price: +document.getElementById('f-price').value,
      status: document.getElementById('f-status').value,
    }),
    fillForm: (row) => {
      document.getElementById('f-name').value = row.name;
      document.getElementById('f-category').value = row.category;
      document.getElementById('f-brand').value = row.brand;
      document.getElementById('f-size').value = row.size;
      document.getElementById('f-color').value = row.color;
      document.getElementById('f-season').value = row.season;
      document.getElementById('f-price').value = row.price;
      document.getElementById('f-status').value = row.status;
    },
  },
  categories: {
    title: 'Категории',
    cols: ['id','name','description'],
    heads: ['#','Название','Описание'],
    stats: () => [
      {label:'Всего категорий', icon:'ti-tag', value:data.categories.length},
      {label:'Товаров всего', icon:'ti-shirt', value:data.products.length},
      {label:'Брендов', icon:'ti-award', value:data.brands.length},
      {label:'В наличии', icon:'ti-check', value:data.products.filter(x=>x.status==='в наличии').length},
    ],
    form: () => `
      <div class="form-group"><label>Название</label><input type="text" id="f-name" placeholder="Название категории"></div>
      <div class="form-group"><label>Описание</label><textarea id="f-description" placeholder="Описание..."></textarea></div>`,
    getValues: () => ({ name:document.getElementById('f-name').value, description:document.getElementById('f-description').value }),
    fillForm: (row) => { document.getElementById('f-name').value=row.name; document.getElementById('f-description').value=row.description; },
  },
  brands: {
    title: 'Бренды',
    cols: ['id','name','country','description'],
    heads: ['#','Название','Страна','Описание'],
    stats: () => [
      {label:'Всего брендов', icon:'ti-award', value:data.brands.length},
      {label:'Стран', icon:'ti-world', value:[...new Set(data.brands.map(b=>b.country))].length},
      {label:'Товаров', icon:'ti-shirt', value:data.products.length},
      {label:'Категорий', icon:'ti-tag', value:data.categories.length},
    ],
    form: () => `
      <div class="form-group"><label>Название</label><input type="text" id="f-name" placeholder="Название бренда"></div>
      <div class="form-row">
        <div class="form-group"><label>Страна</label><input type="text" id="f-country" placeholder="Страна"></div>
      </div>
      <div class="form-group"><label>Описание</label><textarea id="f-description" placeholder="Описание..."></textarea></div>`,
    getValues: () => ({ name:document.getElementById('f-name').value, country:document.getElementById('f-country').value, description:document.getElementById('f-description').value }),
    fillForm: (row) => { document.getElementById('f-name').value=row.name; document.getElementById('f-country').value=row.country; document.getElementById('f-description').value=row.description; },
  },
  customers: {
    title: 'Клиенты',
    cols: ['id','first_name','last_name','phone','email','reg_date','status'],
    heads: ['#','Имя','Фамилия','Телефон','Email','Дата рег.','Статус'],
    badge: 'status',
    badgeMap: { 'постоянный':'success', 'обычный':'info' },
    stats: () => {
      const d = data.customers;
      return [
        {label:'Всего клиентов', icon:'ti-users', value:d.length},
        {label:'Постоянных', icon:'ti-star', value:d.filter(x=>x.status==='постоянный').length},
        {label:'Заказов', icon:'ti-shopping-cart', value:data.orders.length},
        {label:'Выручка', icon:'ti-coin', value:data.orders.reduce((s,o)=>s+o.total,0).toLocaleString('ru')+'₽'},
      ];
    },
    form: () => `
      <div class="form-row">
        <div class="form-group"><label>Имя</label><input type="text" id="f-first_name" placeholder="Имя"></div>
        <div class="form-group"><label>Фамилия</label><input type="text" id="f-last_name" placeholder="Фамилия"></div>
      </div>
      <div class="form-group"><label>Телефон</label><input type="text" id="f-phone" placeholder="+7..."></div>
      <div class="form-group"><label>Email</label><input type="email" id="f-email" placeholder="email@example.ru"></div>
      <div class="form-row">
        <div class="form-group"><label>Дата регистрации</label><input type="date" id="f-reg_date"></div>
        <div class="form-group"><label>Статус</label><select id="f-status"><option>обычный</option><option>постоянный</option></select></div>
      </div>`,
    getValues: () => ({ first_name:document.getElementById('f-first_name').value, last_name:document.getElementById('f-last_name').value, phone:document.getElementById('f-phone').value, email:document.getElementById('f-email').value, reg_date:document.getElementById('f-reg_date').value, status:document.getElementById('f-status').value }),
    fillForm: (row) => { document.getElementById('f-first_name').value=row.first_name; document.getElementById('f-last_name').value=row.last_name; document.getElementById('f-phone').value=row.phone; document.getElementById('f-email').value=row.email; document.getElementById('f-reg_date').value=row.reg_date; document.getElementById('f-status').value=row.status; },
  },
  orders: {
    title: 'Заказы',
    cols: ['id','customer','order_date','status','total'],
    heads: ['#','Клиент','Дата','Статус','Сумма'],
    badge: 'status',
    badgeMap: { 'оплачен':'success', 'новый':'info', 'доставлен':'warning', 'отменён':'danger' },
    price: 'total',
    stats: () => {
      const d = data.orders;
      return [
        {label:'Всего заказов', icon:'ti-shopping-cart', value:d.length},
        {label:'Оплачено', icon:'ti-check', value:d.filter(x=>x.status==='оплачен').length},
        {label:'Выручка', icon:'ti-coin', value:d.filter(x=>x.status==='оплачен').reduce((s,o)=>s+o.total,0).toLocaleString('ru')+'₽'},
        {label:'В обработке', icon:'ti-clock', value:d.filter(x=>x.status==='новый').length},
      ];
    },
    form: () => `
      <div class="form-group"><label>Клиент</label><select id="f-customer">${data.customers.map(c=>`<option>${c.first_name} ${c.last_name}</option>`).join('')}</select></div>
      <div class="form-row">
        <div class="form-group"><label>Дата заказа</label><input type="date" id="f-order_date"></div>
        <div class="form-group"><label>Статус</label><select id="f-status"><option>новый</option><option>оплачен</option><option>доставлен</option><option>отменён</option></select></div>
      </div>
      <div class="form-group"><label>Сумма (₽)</label><input type="number" id="f-total" placeholder="0"></div>`,
    getValues: () => ({ customer:document.getElementById('f-customer').value, order_date:document.getElementById('f-order_date').value, status:document.getElementById('f-status').value, total:+document.getElementById('f-total').value }),
    fillForm: (row) => { document.getElementById('f-customer').value=row.customer; document.getElementById('f-order_date').value=row.order_date; document.getElementById('f-status').value=row.status; document.getElementById('f-total').value=row.total; },
  },
  stock: {
    title: 'Остатки на складе',
    cols: ['id','product','warehouse','quantity','last_update'],
    heads: ['#','Товар','Склад','Кол-во','Обновлено'],
    stats: () => {
      const d = data.stock;
      return [
        {label:'Позиций', icon:'ti-building-warehouse', value:d.length},
        {label:'Всего единиц', icon:'ti-stack', value:d.reduce((s,x)=>s+x.quantity,0)},
        {label:'Нет на складе', icon:'ti-alert-circle', value:d.filter(x=>x.quantity===0).length},
        {label:'Складов', icon:'ti-map-pin', value:[...new Set(d.map(x=>x.warehouse))].length},
      ];
    },
    form: () => `
      <div class="form-group"><label>Товар</label><select id="f-product">${data.products.map(p=>`<option>${p.name}</option>`).join('')}</select></div>
      <div class="form-group"><label>Склад</label><select id="f-warehouse"><option>Основной склад</option><option>Региональный склад</option></select></div>
      <div class="form-row">
        <div class="form-group"><label>Количество</label><input type="number" id="f-quantity" placeholder="0" min="0"></div>
        <div class="form-group"><label>Дата обновления</label><input type="date" id="f-last_update"></div>
      </div>`,
    getValues: () => ({ product:document.getElementById('f-product').value, warehouse:document.getElementById('f-warehouse').value, quantity:+document.getElementById('f-quantity').value, last_update:document.getElementById('f-last_update').value }),
    fillForm: (row) => { document.getElementById('f-product').value=row.product; document.getElementById('f-warehouse').value=row.warehouse; document.getElementById('f-quantity').value=row.quantity; document.getElementById('f-last_update').value=row.last_update; },
  },
  suppliers: {
    title: 'Поставщики',
    cols: ['id','name','contact','phone','email','address'],
    heads: ['#','Название','Контакт','Телефон','Email','Адрес'],
    stats: () => [
      {label:'Поставщиков', icon:'ti-truck', value:data.suppliers.length},
      {label:'Городов', icon:'ti-map-pin', value:[...new Set(data.suppliers.map(s=>s.address))].length},
      {label:'Товаров', icon:'ti-shirt', value:data.products.length},
      {label:'Категорий', icon:'ti-tag', value:data.categories.length},
    ],
    form: () => `
      <div class="form-group"><label>Название компании</label><input type="text" id="f-name" placeholder="ООО..."></div>
      <div class="form-row">
        <div class="form-group"><label>Контактное лицо</label><input type="text" id="f-contact" placeholder="Фамилия И.О."></div>
        <div class="form-group"><label>Телефон</label><input type="text" id="f-phone" placeholder="+7..."></div>
      </div>
      <div class="form-group"><label>Email</label><input type="email" id="f-email" placeholder="email@company.ru"></div>
      <div class="form-group"><label>Адрес</label><input type="text" id="f-address" placeholder="г. Город, ул. Улица..."></div>`,
    getValues: () => ({ name:document.getElementById('f-name').value, contact:document.getElementById('f-contact').value, phone:document.getElementById('f-phone').value, email:document.getElementById('f-email').value, address:document.getElementById('f-address').value }),
    fillForm: (row) => { document.getElementById('f-name').value=row.name; document.getElementById('f-contact').value=row.contact; document.getElementById('f-phone').value=row.phone; document.getElementById('f-email').value=row.email; document.getElementById('f-address').value=row.address; },
  },
  employees: {
    title: 'Сотрудники',
    cols: ['id','first_name','last_name','position','phone','hire_date'],
    heads: ['#','Имя','Фамилия','Должность','Телефон','Дата найма'],
    stats: () => {
      const d = data.employees;
      const pos = [...new Set(d.map(e=>e.position))];
      return [
        {label:'Сотрудников', icon:'ti-id-badge', value:d.length},
        {label:'Должностей', icon:'ti-briefcase', value:pos.length},
        {label:'Клиентов', icon:'ti-users', value:data.customers.length},
        {label:'Заказов', icon:'ti-shopping-cart', value:data.orders.length},
      ];
    },
    form: () => `
      <div class="form-row">
        <div class="form-group"><label>Имя</label><input type="text" id="f-first_name" placeholder="Имя"></div>
        <div class="form-group"><label>Фамилия</label><input type="text" id="f-last_name" placeholder="Фамилия"></div>
      </div>
      <div class="form-group"><label>Должность</label><select id="f-position"><option>Продавец</option><option>Менеджер</option><option>Кладовщик</option><option>Директор</option></select></div>
      <div class="form-row">
        <div class="form-group"><label>Телефон</label><input type="text" id="f-phone" placeholder="+7..."></div>
        <div class="form-group"><label>Дата найма</label><input type="date" id="f-hire_date"></div>
      </div>`,
    getValues: () => ({ first_name:document.getElementById('f-first_name').value, last_name:document.getElementById('f-last_name').value, position:document.getElementById('f-position').value, phone:document.getElementById('f-phone').value, hire_date:document.getElementById('f-hire_date').value }),
    fillForm: (row) => { document.getElementById('f-first_name').value=row.first_name; document.getElementById('f-last_name').value=row.last_name; document.getElementById('f-position').value=row.position; document.getElementById('f-phone').value=row.phone; document.getElementById('f-hire_date').value=row.hire_date; },
  },
};

let currentSection = 'products';
let editingId = null;
let deletingId = null;

function switchSection(section) {
  currentSection = section;
  document.querySelectorAll('.nav-item').forEach(el => el.classList.toggle('active', el.dataset.section === section));
  document.getElementById('page-title').textContent = config[section].title;
  document.getElementById('search-input').value = '';
  renderStats();
  renderTable();
}

function renderStats() {
  const stats = config[currentSection].stats();
  document.getElementById('stats-row').innerHTML = stats.map(s => `
    <div class="stat-card">
      <div class="stat-label"><i class="ti ${s.icon}" aria-hidden="true"></i>${s.label}</div>
      <div class="stat-value">${s.value}</div>
    </div>`).join('');
}

function renderTable(rows) {
  const cfg = config[currentSection];
  const allRows = rows || data[currentSection];
  if (!allRows.length) {
    document.getElementById('table-container').innerHTML = `<div class="empty-state"><i class="ti ti-inbox"></i>Нет данных</div>`;
    return;
  }
  const thead = `<thead><tr>${cfg.heads.map(h=>`<th>${h}</th>`).join('')}<th>Действия</th></tr></thead>`;
  const tbody = `<tbody>${allRows.map(row => {
    const cells = cfg.cols.map(col => {
      let val = row[col];
      if (cfg.badge === col && cfg.badgeMap) {
        const cls = cfg.badgeMap[val] || 'info';
        return `<td><span class="badge badge-${cls}">${val}</span></td>`;
      }
      if (cfg.price === col && typeof val === 'number') return `<td>${val.toLocaleString('ru')}₽</td>`;
      if (col === 'quantity') {
        const color = val === 0 ? 'var(--color-text-danger)' : val < 10 ? 'var(--color-text-warning)' : 'var(--color-text-primary)';
        return `<td style="font-weight:500;color:${color}">${val}</td>`;
      }
      return `<td>${val ?? ''}</td>`;
    }).join('');
    return `<tr>${cells}<td><div class="td-actions">
      <button class="btn-icon" onclick="openEditModal(${row.id})" title="Редактировать"><i class="ti ti-edit"></i></button>
      <button class="btn-icon danger" onclick="openDeleteConfirm(${row.id})" title="Удалить"><i class="ti ti-trash"></i></button>
    </div></td></tr>`;
  }).join('')}</tbody>`;
  document.getElementById('table-container').innerHTML = `<table>${thead}${tbody}</table>`;
}

function filterTable() {
  const q = document.getElementById('search-input').value.toLowerCase();
  if (!q) { renderTable(); return; }
  const filtered = data[currentSection].filter(row =>
    Object.values(row).some(v => String(v).toLowerCase().includes(q))
  );
  renderTable(filtered);
}

function openAddModal() {
  editingId = null;
  document.getElementById('modal-title').textContent = 'Добавить — ' + config[currentSection].title;
  document.getElementById('modal-body').innerHTML = config[currentSection].form();
  document.getElementById('form-overlay').classList.add('open');
}

function openEditModal(id) {
  editingId = id;
  const row = data[currentSection].find(r => r.id === id);
  document.getElementById('modal-title').textContent = 'Редактировать запись';
  document.getElementById('modal-body').innerHTML = config[currentSection].form();
  config[currentSection].fillForm(row);
  document.getElementById('form-overlay').classList.add('open');
}

function closeModal() { document.getElementById('form-overlay').classList.remove('open'); }

function submitForm() {
  const vals = config[currentSection].getValues();
  if (editingId !== null) {
    const idx = data[currentSection].findIndex(r => r.id === editingId);
    data[currentSection][idx] = { ...data[currentSection][idx], ...vals };
  } else {
    const maxId = data[currentSection].reduce((m,r)=>Math.max(m,r.id),0);
    data[currentSection].push({ id: maxId+1, ...vals });
  }
  closeModal();
  renderStats();
  renderTable();
}

function openDeleteConfirm(id) {
  deletingId = id;
  const row = data[currentSection].find(r => r.id === id);
  const name = row.name || (row.first_name ? row.first_name+' '+row.last_name : row.customer || row.product || `#${id}`);
  document.getElementById('confirm-text').textContent = `Удалить «${name}»? Это действие нельзя отменить.`;
  document.getElementById('confirm-overlay').classList.add('open');
}

function closeConfirm() { document.getElementById('confirm-overlay').classList.remove('open'); }

function confirmDelete() {
  data[currentSection] = data[currentSection].filter(r => r.id !== deletingId);
  closeConfirm();
  renderStats();
  renderTable();
}

switchSection('products');
</script>
