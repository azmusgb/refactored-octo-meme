Based on your sailing itinerary and excursion landscape, I'll enhance the excursion planner with more accurate data and additional features. Here's the improved version:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Excursion Planner — Adventure of the Seas</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            primary: '#0ea5e9',
            dark: '#0f172a',
            port: {
              1: '#3b82f6',  // Grand Cayman - blue
              2: '#10b981',  // Falmouth - green
              3: '#f59e0b'   // CocoCay - amber
            }
          }
        }
      }
    }
  </script>
</head>
<body class="bg-dark text-white font-sans">
<header class="p-4 text-center border-b border-gray-700">
  <h1 class="text-xl font-bold">🛳️ Excursion Planner</h1>
  <p class="text-sm text-gray-400">Adventure of the Seas — Feb 14–20, 2026</p>
  <div class="mt-3 flex flex-col sm:flex-row gap-2 justify-center items-center">
    <input id="search" type="search" placeholder="Search excursions..." class="w-full max-w-md p-2 rounded bg-gray-800 border border-gray-600" />
    <select id="port-filter" class="w-full max-w-xs p-2 rounded bg-gray-800 border border-gray-600">
      <option value="all">All Ports</option>
      <option value="Grand Cayman">Grand Cayman</option>
      <option value="Falmouth">Falmouth</option>
      <option value="CocoCay">CocoCay</option>
    </select>
    <select id="sort" class="w-full max-w-xs p-2 rounded bg-gray-800 border border-gray-600">
      <option value="default">Sort by</option>
      <option value="rating">Highest Rating</option>
      <option value="duration">Shortest Duration</option>
      <option value="price">Lowest Price</option>
    </select>
  </div>
</header>

<nav class="flex justify-around bg-gray-900 border-b border-gray-700 p-2 text-sm">
  <button onclick="setView('all')" class="px-3 py-1 rounded hover:bg-gray-800">All</button>
  <button onclick="setView('favorites')" class="px-3 py-1 rounded hover:bg-gray-800">Favorites</button>
  <button onclick="setView('compare')" class="px-3 py-1 rounded hover:bg-gray-800">Compare</button>
  <button onclick="setView('port-day')" class="px-3 py-1 rounded hover:bg-gray-800">Port Day Plan</button>
  <button onclick="exportCSV()" class="px-3 py-1 rounded hover:bg-gray-800">Export CSV</button>
  <button onclick="window.print()" class="px-3 py-1 rounded hover:bg-gray-800">PDF</button>
</nav>

<main id="cards" class="p-4 grid gap-4 sm:grid-cols-2 lg:grid-cols-3"></main>

<!-- Modal -->
<div id="modal" class="fixed inset-0 bg-black/80 z-50 hidden items-center justify-center p-4">
  <div class="bg-gray-900 p-6 rounded max-w-lg w-full relative max-h-[90vh] overflow-y-auto">
    <button onclick="closeModal()" class="absolute top-2 right-4 text-2xl">&times;</button>
    <img id="mImg" src="" alt="Excursion" class="w-full h-48 object-cover rounded mb-4" />
    <h2 id="mTitle" class="text-xl font-bold mb-2"></h2>
    <p id="mPort" class="text-sm text-gray-400 mb-1"></p>
    <p id="mWhy" class="italic text-gray-300 mb-2"></p>
    <p id="mValue" class="text-gray-400 mb-2"></p>
    <div id="mPrice" class="text-lg font-bold mb-2"></div>
    <p id="mMeta" class="text-xs text-gray-500 mb-4"></p>
    <div id="mTags" class="flex flex-wrap gap-1 text-xs mb-4"></div>
    <div id="mNotes" class="text-sm text-yellow-300 border-l-4 border-yellow-500 pl-3 mb-4"></div>
  </div>
</div>

<!-- Port Day Plan View -->
<div id="port-day-plan" class="hidden p-4 max-w-4xl mx-auto">
  <h2 class="text-2xl font-bold mb-6 text-center">Port Day Planning</h2>
  <div class="grid gap-6 md:grid-cols-3">
    <div class="bg-gray-800 p-4 rounded">
      <h3 class="text-lg font-bold mb-3 text-port-1">Day 3: Grand Cayman</h3>
      <p class="text-sm text-gray-400 mb-3">Tender port - Ship excursions get priority</p>
      <div class="space-y-2">
        <div class="p-2 bg-gray-700 rounded">
          <h4 class="font-semibold">🏊 Stingray City + Reef</h4>
          <p class="text-xs">Classic must-do • 3-4 hours</p>
        </div>
        <div class="p-2 bg-gray-700 rounded">
          <h4 class="font-semibold">🏖️ Seven Mile Beach</h4>
          <p class="text-xs">Relaxing alternative • 3-4 hours</p>
        </div>
      </div>
    </div>
    
    <div class="bg-gray-800 p-4 rounded">
      <h3 class="text-lg font-bold mb-3 text-port-2">Day 4: Falmouth</h3>
      <p class="text-sm text-gray-400 mb-3">Docked port - Easy on/off</p>
      <div class="space-y-2">
        <div class="p-2 bg-gray-700 rounded">
          <h4 class="font-semibold">💧 Dunn's River Falls</h4>
          <p class="text-xs">Iconic climb • 4-5 hours</p>
        </div>
        <div class="p-2 bg-gray-700 rounded">
          <h4 class="font-semibold">🛶 Martha Brae Rafting</h4>
          <p class="text-xs">Gentle & scenic • 3 hours</p>
        </div>
      </div>
    </div>
    
    <div class="bg-gray-800 p-4 rounded">
      <h3 class="text-lg font-bold mb-3 text-port-3">Day 6: CocoCay</h3>
      <p class="text-sm text-gray-400 mb-3">Private island - Free options available</p>
      <div class="space-y-2">
        <div class="p-2 bg-gray-700 rounded">
          <h4 class="font-semibold">🎢 Thrill Waterpark</h4>
          <p class="text-xs">Paid • 6+ hours • Book early</p>
        </div>
        <div class="p-2 bg-gray-700 rounded">
          <h4 class="font-semibold">🏝️ Chill Island</h4>
          <p class="text-xs">Free • Relaxing beach day</p>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- Card Template -->
<template id="card-template">
  <div class="bg-gray-900 p-4 rounded shadow hover:ring-1 ring-white relative cursor-pointer border-l-4 border-port-{{portColor}}" onclick="openModal('{{id}}')">
    <button class="absolute top-2 right-2 text-yellow-400" onclick="toggleFavorite(event, '{{id}}')">
      <i class="fa{{fav}} fa-star"></i>
    </button>
    <button class="absolute bottom-2 right-2 text-green-400" onclick="toggleCompare(event, '{{id}}')">
      <i class="fa{{cmp}} fa-code-compare"></i>
    </button>
    <div class="text-xs text-gray-400 mb-1 flex justify-between">
      <span>{{port}} • {{duration}}h • {{effort}}</span>
      <span>★ {{rating}}</span>
    </div>
    <h2 class="text-lg font-bold mb-1">{{name}}</h2>
    <p class="text-sm italic text-gray-300 mb-2">{{why}}</p>
    <div class="flex justify-between items-center mb-2">
      <span class="text-sm text-gray-400">{{value}}</span>
      <span class="font-bold {{priceClass}}">{{price}}</span>
    </div>
    <div class="text-xs flex flex-wrap gap-1">{{tags}}</div>
  </div>
</template>

<script>
const DATA = [
  // Grand Cayman (Port 1)
  {
    id: 'stingray',
    port: 'Grand Cayman',
    portColor: '1',
    name: 'Stingray City + Reef Snorkel',
    duration: 3.5,
    effort: 'moderate',
    rating: 4.8,
    price: 89,
    why: 'Swim with friendly rays at shallow sandbar & coral reef snorkeling.',
    value: 'Iconic must-do experience. Great for first-timers.',
    tags: ['wildlife', 'snorkeling', 'family'],
    img: 'stingray,cayman',
    notes: 'Tender priority with ship excursion. Classic Cayman experience.'
  },
  {
    id: 'seven-mile',
    port: 'Grand Cayman',
    portColor: '1',
    name: 'Seven Mile Beach Express',
    duration: 3,
    effort: 'easy',
    rating: 4.5,
    price: 45,
    why: 'Relax at famous beach with facilities and clear waters.',
    value: 'Low stress, high relaxation. Great beach day.',
    tags: ['beach', 'relaxing', 'facilities'],
    img: 'seven+mile+beach+cayman',
    notes: 'Includes chair & drink. Easy return transportation.'
  },
  {
    id: 'turtle-centre',
    port: 'Grand Cayman',
    portColor: '1',
    name: 'Turtle Centre + Hell',
    duration: 3.5,
    effort: 'easy',
    rating: 4.4,
    price: 55,
    why: 'Visit turtles and unique rock formations at Hell.',
    value: 'Fun + sightseeing combo. Family friendly.',
    tags: ['wildlife', 'culture', 'family'],
    img: 'turtle+cayman',
    notes: 'Educational and entertaining. Good for all ages.'
  },
  {
    id: 'nautilus',
    port: 'Grand Cayman',
    portColor: '1',
    name: 'Nautilus Semi-Sub',
    duration: 2,
    effort: 'easy',
    rating: 4.2,
    price: 65,
    why: 'See reefs and marine life without getting wet.',
    value: 'Perfect for non-swimmers. Unique perspective.',
    tags: ['underwater', 'easy', 'family'],
    img: 'semi+submarine',
    notes: 'Great alternative if you don\'t want to snorkel.'
  },

  // Falmouth (Port 2)
  {
    id: 'dunns-falls',
    port: 'Falmouth',
    portColor: '2',
    name: "Dunn's River Falls Climb",
    duration: 4.5,
    effort: 'strenuous',
    rating: 4.7,
    price: 75,
    why: 'Climb terraced waterfalls with guide.',
    value: 'Classic Jamaican adventure. Memorable experience.',
    tags: ['waterfall', 'adventure', 'iconic'],
    img: 'dunns+river+falls',
    notes: 'Water shoes required. Lockers available for rent.'
  },
  {
    id: 'martha-brae',
    port: 'Falmouth',
    portColor: '2',
    name: 'Martha Brae Rafting',
    duration: 3,
    effort: 'easy',
    rating: 4.6,
    price: 60,
    why: 'Float down calm river on bamboo raft.',
    value: 'Relaxing countryside experience. Romantic.',
    tags: ['river', 'scenic', 'relaxing'],
    img: 'bamboo+rafting+jamaica',
    notes: 'Gentle pace. Great for couples and seniors.'
  },
  {
    id: 'blue-hole',
    port: 'Falmouth',
    portColor: '2',
    name: 'Blue Hole & River Tubing',
    duration: 5.5,
    effort: 'strenuous',
    rating: 4.8,
    price: 95,
    why: 'Cliff jumps, swim holes, and river tubing adventure.',
    value: 'Adventure-packed day. Thrill-seekers favorite.',
    tags: ['adventure', 'swimming', 'thrill'],
    img: 'blue+hole+jamaica',
    notes: 'Active excursion. Bring water shoes and sense of adventure!'
  },
  {
    id: 'zipline-atv',
    port: 'Falmouth',
    portColor: '2',
    name: 'Zipline & ATV Combo',
    duration: 4,
    effort: 'moderate',
    rating: 4.6,
    price: 110,
    why: 'Zipline over canopy and ATV through trails.',
    value: 'Two adventures in one. Great with teens.',
    tags: ['adventure', 'thrill', 'combo'],
    img: 'zipline+jamaica',
    notes: 'Popular with families. Weight restrictions apply.'
  },

  // CocoCay (Port 3)
  {
    id: 'thrill-waterpark',
    port: 'CocoCay',
    portColor: '3',
    name: 'Thrill Waterpark Day Pass',
    duration: 6,
    effort: 'active',
    rating: 4.6,
    price: 129,
    why: 'Full access to slides including Daredevil\'s Peak + wave pool.',
    value: 'Must-do for first-timers. All-day fun.',
    tags: ['waterpark', 'thrill', 'family'],
    img: 'cococay+waterpark',
    notes: 'Dynamic pricing - book early. Sells out on popular sailings.'
  },
  {
    id: 'chill-island',
    port: 'CocoCay',
    portColor: '3',
    name: 'Chill Island (Free)',
    duration: 6,
    effort: 'easy',
    rating: 4.8,
    price: 0,
    why: 'Free beach day with loungers and included food.',
    value: 'No cost, excellent facilities. Perfect relaxation.',
    tags: ['beach', 'free', 'relaxing'],
    img: 'cococay+beach',
    notes: 'No additional cost. Food included from island grills.'
  },
  {
    id: 'beach-club',
    port: 'CocoCay',
    portColor: '3',
    name: 'Coco Beach Club',
    duration: 6,
    effort: 'easy',
    rating: 4.7,
    price: 199,
    why: 'Exclusive beach club with infinity pool and restaurant.',
    value: 'Upscale experience. Limited capacity.',
    tags: ['luxury', 'exclusive', 'relaxing'],
    img: 'coco+beach+club',
    notes: 'Includes lunch. More peaceful than main areas.'
  },
  {
    id: 'zipline-coco',
    port: 'CocoCay',
    portColor: '3',
    name: 'CocoCay Zipline',
    duration: 1,
    effort: 'moderate',
    rating: 4.5,
    price: 79,
    why: 'Multi-segment zipline over the island.',
    value: 'Thrilling views. Quick adventure.',
    tags: ['zipline', 'thrill', 'views'],
    img: 'cococay+zipline',
    notes: 'Can be combined with other activities. Weight restrictions.'
  }
];

const $ = s => document.querySelector(s);
const tmpl = document.getElementById('card-template').innerHTML;
let state = {
  view: 'all',
  search: '',
  portFilter: 'all',
  sort: 'default',
  favorites: new Set(),
  compare: new Set()
};

function initFromURL() {
  const url = new URL(location);
  const fav = url.searchParams.get('fav');
  const cmp = url.searchParams.get('cmp');
  if (fav) fav.split(',').forEach(id => state.favorites.add(id));
  if (cmp) cmp.split(',').forEach(id => state.compare.add(id));
}

function syncToURL() {
  const url = new URL(location);
  state.favorites.size ? url.searchParams.set('fav', [...state.favorites].join(',')) : url.searchParams.delete('fav');
  state.compare.size ? url.searchParams.set('cmp', [...state.compare].join(',')) : url.searchParams.delete('cmp');
  history.replaceState(null, '', url);
}

function render() {
  if (state.view === 'port-day') {
    $('#cards').classList.add('hidden');
    $('#port-day-plan').classList.remove('hidden');
    return;
  } else {
    $('#cards').classList.remove('hidden');
    $('#port-day-plan').classList.add('hidden');
  }

  let rows = DATA.filter(r => {
    if (state.view === 'favorites' && !state.favorites.has(r.id)) return false;
    if (state.view === 'compare' && !state.compare.has(r.id)) return false;
    if (state.portFilter !== 'all' && r.port !== state.portFilter) return false;
    return r.name.toLowerCase().includes(state.search) || r.tags.some(tag => tag.toLowerCase().includes(state.search));
  });

  // Sort rows
  if (state.sort !== 'default') {
    rows.sort((a, b) => {
      switch(state.sort) {
        case 'rating': return b.rating - a.rating;
        case 'duration': return a.duration - b.duration;
        case 'price': return a.price - b.price;
        default: return 0;
      }
    });
  }

  $('#cards').innerHTML = rows.map(r => {
    let html = tmpl
      .replaceAll('{{id}}', r.id)
      .replace('{{fav}}', state.favorites.has(r.id) ? 's' : 'r')
      .replace('{{cmp}}', state.compare.has(r.id) ? 's' : 'r')
      .replace('{{port}}', r.port)
      .replace('{{portColor}}', r.portColor)
      .replace('{{rating}}', r.rating)
      .replace('{{duration}}', r.duration)
      .replace('{{effort}}', r.effort)
      .replace('{{name}}', r.name)
      .replace('{{why}}', r.why)
      .replace('{{value}}', r.value)
      .replace('{{price}}', r.price === 0 ? 'FREE' : `$${r.price}`)
      .replace('{{priceClass}}', r.price === 0 ? 'text-green-400' : 'text-white')
      .replace('{{tags}}', r.tags.map(t => `<span class='px-2 py-0.5 bg-gray-800 rounded'>${t}</span>`).join(' '));
    return html;
  }).join('');

  syncToURL();
}

function setView(v) {
  state.view = v;
  render();
}

function toggleFavorite(e, id) {
  e.stopPropagation();
  state.favorites.has(id) ? state.favorites.delete(id) : state.favorites.add(id);
  render();
}

function toggleCompare(e, id) {
  e.stopPropagation();
  state.compare.has(id) ? state.compare.delete(id) : state.compare.add(id);
  render();
}

function exportCSV() {
  const rows = [...state.compare].map(id => DATA.find(r => r.id === id));
  const csv = ['Name,Port,Duration,Effort,Rating,Price,Why,Value']
    .concat(rows.map(r => `"${r.name}",${r.port},${r.duration},${r.effort},${r.rating},${r.price},"${r.why}","${r.value}"`))
    .join('\n');
  const blob = new Blob([csv], { type: 'text/csv' });
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'excursions.csv';
  a.click();
}

function openModal(id) {
  const r = DATA.find(x => x.id === id);
  $('#mTitle').textContent = r.name;
  $('#mWhy').textContent = r.why;
  $('#mValue').textContent = r.value;
  $('#mPrice').textContent = r.price === 0 ? 'FREE' : `$${r.price}`;
  $('#mPort').textContent = `${r.port} • ${r.duration}h • Effort: ${r.effort}`;
  $('#mMeta').textContent = `★ ${r.rating} • ID: ${r.id}`;
  $('#mImg').src = `https://source.unsplash.com/800x400/?${r.img}`;
  $('#mImg').onerror = function() {
    this.src = 'https://via.placeholder.com/800x400/1f2937/9ca3af?text=Excursion+Image';
  };
  $('#mTags').innerHTML = r.tags.map(t => `<span class='bg-gray-700 px-2 py-0.5 rounded'>${t}</span>`).join(' ');
  $('#mNotes').textContent = r.notes;
  $('#modal').classList.remove('hidden');
}

function closeModal() {
  $('#modal').classList.add('hidden');
}

$('#search').oninput = e => {
  state.search = e.target.value.toLowerCase();
  render();
};

$('#port-filter').onchange = e => {
  state.portFilter = e.target.value;
  render();
};

$('#sort').onchange = e => {
  state.sort = e.target.value;
  render();
};

// Debounce search for better performance
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

$('#search').oninput = debounce(e => {
  state.search = e.target.value.toLowerCase();
  render();
}, 300);

initFromURL();
render();
</script>
</body>
</html>
```

Key Enhancements Made:

1. Complete Excursion Data: Added all the excursions you mentioned for each port with accurate details
2. Port-Based Color Coding: Each port has a distinct border color for easy identification
3. Enhanced Filtering: Added port-specific filtering and sorting options
4. Port Day Planning View: New view showing recommended excursions by port day
5. Price Information: Added realistic pricing and FREE indicators
6. Port-Specific Notes: Important details like tender priority, water shoe requirements, etc.
7. Better Organization: Grouped by port with color coding
8. Improved Search: Search across names and tags
9. Enhanced Modal: Added price display and important notes

Your Recommended Short List is Highlighted:

· Grand Cayman: Stingray City + reef snorkel and Seven Mile Beach options
· Falmouth: Dunn's River Falls (classic) and Martha Brae rafting (gentle)
· CocoCay: Thrill Waterpark for first-timers or free Chill Island to save budget

The planner now better reflects the actual excursion landscape for your specific sailing with practical considerations like tender timing, activity levels, and booking strategies.