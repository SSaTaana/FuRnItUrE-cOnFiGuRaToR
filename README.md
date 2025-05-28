<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Конфигуратор мебельной фурнитуры</title>
  <script src="https://cdn.jsdelivr.net/npm/react@18.2.0/umd/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/react-dom@18.2.0/umd/react-dom.development.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/react-dom@18.2.0/client.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/babel-standalone@6.26.0/babel.min.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/pdfmake/0.1.68/pdfmake.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/pdfmake/0.1.68/vfs_fonts.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/i18next@23.11.5/dist/umd/i18next.min.js"></script>
  <style>
    body {
      background: linear-gradient(135deg, #0d1b2a, #1b263b);
      min-height: 100vh;
      font-family: 'Arial', sans-serif;
      color: #e0e1dd;
      position: relative;
      overflow-x: hidden;
    }
    .container-overlay, .welcome-container {
      background: rgba(255, 255, 255, 0.1);
      border-radius: 1.5rem;
      padding: 2rem;
      box-shadow: 0 15px 40px rgba(0, 0, 0, 0.8);
      backdrop-filter: blur(10px);
      max-width: 90rem;
      width: 100%;
      margin: 2rem auto;
      transition: transform 0.5s ease, opacity 0.5s ease;
    }
    .welcome-container {
      max-width: 45rem;
      text-align: center;
      animation: fadeInSlide 1s ease-out;
    }
    .configurator-container {
      animation: fadeInUp 1s ease-out;
    }
    .edge-animation {
      position: fixed;
      top: 0;
      bottom: 0;
      width: 60px;
      background: linear-gradient(90deg, rgba(0, 255, 150, 0.3), transparent);
      animation: edgePulse 4s ease-in-out infinite;
      pointer-events: none;
    }
    .edge-animation.left { left: 0; }
    .edge-animation.right { right: 0; background: linear-gradient(270deg, rgba(138, 43, 226, 0.3), transparent); }
    .navbar { position: fixed; top: 0; width: 100%; background: linear-gradient(90deg, #0d1b2a, #415a77); padding: 1.5rem 0; z-index: 20; box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5); }
    .price-inputs-container { display: flex; flex-direction: row; gap: 2rem; align-items: flex-start; margin-bottom: 2rem; }
    .tab-buttons { display: flex; gap: 1rem; align-items: center; z-index: 10; }
    .tab-button { background: rgba(255, 255, 255, 0.15); border-radius: 0.75rem; padding: 0.75rem 1.5rem; color: #e0e1dd; cursor: pointer; transition: all 0.3s ease; font-weight: 600; }
    .tab-button.active { background: linear-gradient(45deg, #00ff96, #8a2be2); color: #fff; transform: scale(1.05); }
    .tab-content { margin-top: 7rem; }
    .tooltip .tooltip-text { visibility: hidden; width: 220px; background: rgba(0, 0, 0, 0.7); color: #d1e8e2; text-align: center; border-radius: 0.5rem; padding: 0.5rem; position: absolute; z-index: 1; left: -240px; top: 50%; transform: translateY(-50%); opacity: 0; transition: opacity 0.3s ease; }
    .tooltip:hover .tooltip-text { visibility: visible; opacity: 0.95; }
    @keyframes fadeInSlide { 0% { opacity: 0; transform: translateY(20px); } 100% { opacity: 1; transform: translateY(0); } }
    @keyframes fadeInUp { 0% { opacity: 0; transform: translateY(30px); } 100% { opacity: 1; transform: translateY(0); } }
    @keyframes slideUp { 0% { opacity: 0; transform: translateY(50px); } 100% { opacity: 1; transform: translateY(0); } }
    @keyframes tourFadeIn { 0% { opacity: 0; transform: scale(0.9); } 100% { opacity: 1; transform: scale(1); } }
    @keyframes edgePulse { 0% { opacity: 0.4; } 50% { opacity: 0.9; } 100% { opacity: 0.4; } }
    @media (max-width: 640px) {
      .container-overlay, .welcome-container { padding: 1rem; margin: 3rem 0.5rem; }
      .welcome-container { margin: 5rem 0.5rem; }
      h1 { font-size: 1.8rem; }
      h2 { font-size: 1.3rem; }
      .grid-cols-3 { grid-template-columns: 1fr; }
      .edge-animation { display: none; }
      .navbar { padding: 0.75rem 0; }
      .navbar a { font-size: 0.95rem; padding: 0.5rem; }
      .price-inputs-container { flex-direction: column; align-items: center; }
      .tab-buttons { flex-direction: column; width: 100%; margin-top: 1rem; }
      .tab-button { width: 100%; text-align: center; padding: 0.5rem; font-size: 0.95rem; }
      .tab-content { margin-top: 5rem; }
      select, input { font-size: 1rem; padding: 0.75rem; }
      button { font-size: 1rem; padding: 0.75rem; }
    }
    .about-container { background: linear-gradient(135deg, rgba(0, 255, 150, 0.2), rgba(0, 0, 0, 0.9)); border-radius: 1.5rem; padding: 2.5rem; box-shadow: 0 15px 40px rgba(0, 0, 0, 0.8); text-align: center; max-width: 55rem; margin: 7rem auto; animation: fadeInUp 1s ease-out; }
    .about-container h2 { font-size: 2.2rem; color: #00ff96; margin-bottom: 1.5rem; }
    .about-container p { font-size: 1.2rem; color: #d1e8e2; line-height: 1.7; }
    .promotion-card { background: rgba(255, 255, 255, 0.1); border-radius: 1rem; padding: 1.5rem; margin: 1rem 0; transition: transform 0.4s ease, box-shadow 0.4s ease; animation: slideUp 0.8s ease-out; }
    .promotion-card:hover { transform: translateY(-10px) rotate(2deg); box-shadow: 0 15px 30px rgba(0, 255, 150, 0.3); }
    .tour-container { max-width: 90rem; margin: 7rem auto; text-align: center; opacity: 0; animation: tourFadeIn 1s ease-out forwards; animation-delay: 0.2s; }
    .tour-container iframe { width: 100%; height: 450px; border-radius: 1.5rem; box-shadow: 0 15px 40px rgba(0, 0, 0, 0.8); transition: transform 0.5s ease; }
    .tour-container:hover iframe { transform: scale(1.02); }
    @media (max-width: 640px) {
      .tour-container { margin: 5rem 0.5rem; }
      .tour-container iframe { height: 300px; }
    }
  </style>
</head>
<body className="flex flex-col min-h-screen p-4">
  <div className="edge-animation left"></div>
  <div className="edge-animation right"></div>
  <div id="root"></div>
  <script type="text/babel">
    const { useState, useEffect } = React;
    const { createRoot } = ReactDOM;

    i18next.init({
      lng: 'ru',
      resources: {
        ru: {
          translation: {
            "welcome_title": "Добро пожаловать!",
            "welcome_text": "Откройте мир мебельной фурнитуры с нашим удобным конфигуратором.",
            "go_to_configurator": "Начать настройку",
            "configurator_title": "Конфигуратор фурнитуры",
            "type_label": "Тип фурнитуры",
            "type_placeholder": "Выберите тип",
            "type_tooltip": "Выберите тип, например, петли или ручки.",
            "brand_label": "Бренд",
            "brand_placeholder": "Выберите бренд",
            "brand_tooltip": "Выберите бренд, например, GTV или BOYARD.",
            "specific_option_label": "Опции",
            "specific_option_placeholder": "Выберите опцию",
            "specific_option_tooltip": "Уточните характеристики, например, доводчик.",
            "price_range_from": "Цена от (руб.)",
            "price_range_to": "Цена до (руб.)",
            "find_options": "Найти",
            "results_title": "Найденные варианты:",
            "tab_results": "Результаты",
            "tab_saved": "Сохранённые",
            "save": "Сохранить",
            "saved_results_title": "Сохранённые варианты:",
            "export_selected_pdf": "Экспорт выбранных",
            "export_all_pdf": "Экспорт всех",
            "no_match": "Совпадений нет. Показываем похожие.",
            "price_disclaimer": "*Цена может варьироваться",
            "about_title": "О нас",
            "about_text": "Мы предлагаем инновационный конфигуратор для выбора фурнитуры с актуальными данными.",
            "nav_home": "Главная",
            "nav_configurator": "Конфигуратор",
            "nav_about": "О нас",
            "nav_warehouse": "Виртуальный тур",
            "nav_promotions": "Акции",
            "nav_contacts": "Контакты",
            "nav_reviews": "Отзывы",
            "lang_ru": "Русский",
            "lang_en": "English",
            "name_label": "Наименование",
            "type": "Тип",
            "subtype_label": "Подтип",
            "option": "Опция",
            "brand": "Бренд",
            "price": "Цена",
            "description": "Описание",
            "promotions_title": "Акции и скидки",
            "promotion_gtv_discount": "Скидка 20% на GTV до 2025-06-15",
            "promotion_free_delivery": "Бесплатная доставка от 7000 руб. до 2025-06-20",
            "warehouse_title": "Виртуальный тур по складу",
            "warehouse_text": "Погрузитесь в мир нашей фурнитуры!",
            "contacts_title": "Свяжитесь с нами",
            "contacts_text": "Оставьте заявку для консультации.",
            "reviews_title": "Отзывы клиентов",
            "reviews_text": "Поделитесь своим опытом!",
            "sort_asc": "Сортировать по возрастанию",
            "sort_desc": "Сортировать по убыванию"
          }
        },
        en: {
          translation: {
            "welcome_title": "Welcome!",
            "welcome_text": "Discover the world of furniture fittings with our configurator.",
            "go_to_configurator": "Start Configuration",
            "configurator_title": "Fittings Configurator",
            "type_label": "Fitting Type",
            "type_placeholder": "Select type",
            "type_tooltip": "Select a type, e.g., hinges or handles.",
            "brand_label": "Brand",
            "brand_placeholder": "Select brand",
            "brand_tooltip": "Select a brand, e.g., GTV or BOYARD.",
            "specific_option_label": "Options",
            "specific_option_placeholder": "Select option",
            "specific_option_tooltip": "Specify features, e.g., soft-close.",
            "price_range_from": "Price from (RUB)",
            "price_range_to": "Price to (RUB)",
            "find_options": "Find",
            "results_title": "Found Options:",
            "tab_results": "Results",
            "tab_saved": "Saved",
            "save": "Save",
            "saved_results_title": "Saved Options:",
            "export_selected_pdf": "Export Selected",
            "export_all_pdf": "Export All",
            "no_match": "No matches. Showing similar.",
            "price_disclaimer": "*Price may vary",
            "about_title": "About Us",
            "about_text": "We offer an innovative configurator with up-to-date fitting data.",
            "nav_home": "Home",
            "nav_configurator": "Configurator",
            "nav_about": "About Us",
            "nav_warehouse": "Virtual Tour",
            "nav_promotions": "Promotions",
            "nav_contacts": "Contacts",
            "nav_reviews": "Reviews",
            "lang_ru": "Русский",
            "lang_en": "English",
            "name_label": "Name",
            "type": "Type",
            "subtype_label": "Subtype",
            "option": "Option",
            "brand": "Brand",
            "price": "Price",
            "description": "Description",
            "promotions_title": "Promotions and Discounts",
            "promotion_gtv_discount": "20% off GTV until 2025-06-15",
            "promotion_free_delivery": "Free delivery over 7000 RUB until 2025-06-20",
            "warehouse_title": "Virtual Warehouse Tour",
            "warehouse_text": "Dive into our fittings world!",
            "contacts_title": "Contact Us",
            "contacts_text": "Leave a request for consultation.",
            "reviews_title": "Customer Reviews",
            "reviews_text": "Share your experience!",
            "sort_asc": "Sort Ascending",
            "sort_desc": "Sort Descending"
          }
        }
      }
    });

    const Navbar = ({ setCurrentPage, onLanguageChange }) => {
      const changeLanguage = (lng) => {
        i18next.changeLanguage(lng);
        onLanguageChange(lng);
        document.querySelectorAll('[data-i18n]').forEach(elem => {
          elem.textContent = i18next.t(elem.getAttribute('data-i18n'));
        });
      };

      return (
        React.createElement('nav', { className: 'navbar flex justify-center gap-6' },
          React.createElement('a', {
            href: '#',
            onClick: () => setCurrentPage('welcome'),
            className: 'text-white hover:text-green-400 transition duration-300',
            'data-i18n': 'nav_home'
          }, i18next.t('nav_home')),
          React.createElement('a', {
            href: '#',
            onClick: () => setCurrentPage('configurator'),
            className: 'text-white hover:text-green-400 transition duration-300',
            'data-i18n': 'nav_configurator'
          }, i18next.t('nav_configurator')),
          React.createElement('a', {
            href: '#',
            onClick: () => setCurrentPage('about'),
            className: 'text-white hover:text-green-400 transition duration-300',
            'data-i18n': 'nav_about'
          }, i18next.t('nav_about')),
          React.createElement('a', {
            href: '#',
            onClick: () => setCurrentPage('warehouse'),
            className: 'text-white hover:text-green-400 transition duration-300',
            'data-i18n': 'nav_warehouse'
          }, i18next.t('nav_warehouse')),
          React.createElement('a', {
            href: '#',
            onClick: () => setCurrentPage('promotions'),
            className: 'text-white hover:text-green-400 transition duration-300',
            'data-i18n': 'nav_promotions'
          }, i18next.t('nav_promotions')),
          React.createElement('a', {
            href: '#',
            onClick: () => setCurrentPage('contacts'),
            className: 'text-white hover:text-green-400 transition duration-300',
            'data-i18n': 'nav_contacts'
          }, i18next.t('nav_contacts')),
          React.createElement('a', {
            href: '#',
            onClick: () => setCurrentPage('reviews'),
            className: 'text-white hover:text-green-400 transition duration-300',
            'data-i18n': 'nav_reviews'
          }, i18next.t('nav_reviews')),
          React.createElement('div', { className: 'flex gap-2' },
            React.createElement('button', {
              onClick: () => changeLanguage('ru'),
              className: 'text-white hover:text-green-400 transition duration-300',
              'data-i18n': 'lang_ru'
            }, i18next.t('lang_ru')),
            React.createElement('button', {
              onClick: () => changeLanguage('en'),
              className: 'text-white hover:text-green-400 transition duration-300',
              'data-i18n': 'lang_en'
            }, i18next.t('lang_en'))
          )
        )
      );
    };

    const WelcomePage = ({ onNavigate }) => {
      return (
        React.createElement('div', { className: 'welcome-container' },
          React.createElement('h1', {
            className: 'text-5xl font-bold mb-8 bg-gradient-to-r from-green-400 to-purple-600 bg-clip-text text-transparent',
            'data-i18n': 'welcome_title'
          }, i18next.t('welcome_title')),
          React.createElement('p', { className: 'text-gray-300 mb-10', 'data-i18n': 'welcome_text' }, i18next.t('welcome_text')),
          React.createElement('button', {
            onClick: onNavigate,
            className: 'bg-gradient-to-r from-green-500 to-purple-600 text-white px-8 py-4 rounded-xl shadow-lg hover:from-green-600 hover:to-purple-700 transition duration-300 transform hover:scale-110',
            'data-i18n': 'go_to_configurator'
          }, i18next.t('go_to_configurator'))
        )
      );
    };

    const ConfiguratorPage = ({ language }) => {
      const [type, setType] = useState('');
      const [specificOption, setSpecificOption] = useState('');
      const [brand, setBrand] = useState('');
      const [priceRange, setPriceRange] = useState({ min: '', max: '' });
      const [results, setResults] = useState([]);
      const [savedResults, setSavedResults] = useState([]);
      const [activeTab, setActiveTab] = useState('results');
      const [error, setError] = useState('');
      const [sortOrder, setSortOrder] = useState('none');

      const fittingsDatabase = [
        { id: 1, name: "Петля угловая стандартная", type: "Петля", subtype: "Угловая", mechanism: "Без доводчика", specificOption: "Стандартная", brand: "GTV", price: 500, description: "Надёжная угловая петля для шкафов.", image: "https://avatars.mds.yandex.net/i?id=6c6f936f9f225749da710eacb5cccc32426fde80-8132087-images-thumbs&n=13" },
        { id: 2, name: "Петля накладная с доводчиком", type: "Петля", subtype: "Накладная", mechanism: "С доводчиком", specificOption: "С доводчиком", brand: "BOYARD", price: 800, description: "Накладная петля с плавным закрыванием.", image: "https://www.boyard.biz/thumbs/original/products/h301a020930/36e376c9887d25fa4c777f18305719e2.jpg" },
        { id: 3, name: "Петля врезная push-to-open", type: "Петля", subtype: "Врезная", mechanism: "Push-to-open", specificOption: "Push-to-open", brand: "DECOLINE", price: 1200, description: "Врезная петля с push-механизмом.", image: "https://gratis72.ru/upload/iblock/493/tehvfjk7lpyrz06nq7qb77sv6lj4drk1/08558114_fe74_11ec_b964_00155d936a00_5a75ba8f_fe7c_11ec_b964_00155d936a00.png" },
        { id: 4, name: "Направляющая шариковая стандартная", type: "Направляющая", subtype: "Шариковая", mechanism: "Без доводчика", specificOption: "Стандартная", brand: "MF", price: 1500, description: "Шариковая направляющая для ящиков.", image: "https://avatars.mds.yandex.net/i?id=f5e609961ac0460c78fd86fec8d1fcbfe10626fd-5217037-images-thumbs&n=13" },
        { id: 5, name: "Направляющая роликовая с доводчиком", type: "Направляющая", subtype: "Роликовая", mechanism: "С доводчиком", specificOption: "С доводчиком", brand: "GTV", price: 1800, description: "Роликовая направляющая с плавным ходом.", image: "https://cdn1.ozone.ru/s3/multimedia-b/6042782627.jpg" },
        { id: 6, name: "Ручка круглая классическая", type: "Ручка", subtype: "Круглая", mechanism: "Без доводчика", specificOption: "Классическая", brand: "BOYARD", price: 300, description: "Классическая круглая ручка.", image: "https://avatars.mds.yandex.net/i?id=0162c28c527b7eb6053a30b1b042ed7b_l-5233220-images-thumbs&n=13" },
        { id: 7, name: "Ручка длинная современная", type: "Ручка", subtype: "Длинная", mechanism: "Без доводчика", specificOption: "Рейлинг", brand: "DECOLINE", price: 400, description: "Современная длинная ручка-рейлинг.", image: "https://avatars.mds.yandex.net/i?id=2b0da45dcaa2a51019f95bb92fa43d4d_l-5859311-images-thumbs&n=13" },
        { id: 8, name: "Крепление угловое базовое", type: "Крепление", subtype: "Угловое", mechanism: "Без доводчика", specificOption: "Базовое", brand: "MF", price: 400, description: "Базовое угловое крепление.", image: "https://avatars.mds.yandex.net/i?id=e7033f6d7e9ef1a87d8b637230e2c403_l-3986726-images-thumbs&n=13" },
        { id: 9, name: "Крепление скрытое регулируемое", type: "Крепление", subtype: "Скрытое", mechanism: "Без доводчика", specificOption: "Регулируемое", brand: "GTV", price: 500, description: "Регулируемое скрытое крепление.", image: "https://avatars.mds.yandex.net/i?id=dd500551077a730b1cf29678cfb273c1a3da9594-4012435-images-thumbs&n=13" },
        { id: 10, name: "Петля угловая усиленная", type: "Петля", subtype: "Угловая", mechanism: "Без доводчика", specificOption: "Усиленная", brand: "BOYARD", price: 600, description: "Усиленная угловая петля.", image: "https://avatars.mds.yandex.net/i?id=4769194da1fc6493664b7a7fb8e62386_l-5220849-images-thumbs&n=13" }
      ];

      const specificOptions = {
        "Петля": ["Стандартная", "С доводчиком", "Push-to-open", "Усиленная"],
        "Направляющая": ["Стандартная", "С доводчиком", "Лёгкая"],
        "Ручка": ["Классическая", "Рейлинг"],
        "Крепление": ["Базовое", "Регулируемое"]
      };

      const findBestOptions = () => {
        if (!fittingsDatabase || fittingsDatabase.length === 0) {
          setError(i18next.t('no_match'));
          return;
        }

        const filteredFittings = fittingsDatabase.filter(fitting => {
          const typeMatch = type ? fitting.type === type : true;
          const specificOptionMatch = specificOption ? fitting.specificOption === specificOption : true;
          const brandMatch = brand ? fitting.brand === brand : true;
          const priceMinMatch = priceRange.min !== '' ? fitting.price >= Number(priceRange.min) : true;
          const priceMaxMatch = priceRange.max !== '' ? fitting.price <= Number(priceRange.max) : true;

          return typeMatch && specificOptionMatch && brandMatch && priceMinMatch && priceMaxMatch;
        });

        if (filteredFittings.length === 0) {
          setError(i18next.t('no_match'));
          const fallback = type ? fittingsDatabase.find(f => f.type === type) : fittingsDatabase[0];
          setResults(fallback ? [fallback] : []);
        } else {
          setError('');
          setResults(filteredFittings);
        }
      };

      const saveToPDF = (allResults = false) => {
        const content = allResults ? [...results, ...savedResults] : results;
        const docDefinition = {
          content: [
            { text: allResults ? i18next.t('export_all_pdf') : i18next.t('export_selected_pdf'), style: 'header' },
            ...content.map(fitting => ({
              text: [
                `${i18next.t('name_label')}: ${fitting.name}\n`,
                `${i18next.t('type')}: ${fitting.type} (${fitting.subtype})\n`,
                `${i18next.t('option')}: ${fitting.specificOption}\n`,
                `${i18next.t('brand')}: ${fitting.brand}\n`,
                `${i18next.t('price')}: ${fitting.price} руб.\n`,
                `${i18next.t('description')}: ${fitting.description}\n\n`
              ]
            }))
          ],
          styles: {
            header: { fontSize: 18, bold: true, margin: [0, 0, 0, 10], color: '#000000' }
          }
        };
        pdfMake.createPdf(docDefinition).download(`fittings_results_${allResults ? 'all' : 'selected'}.pdf`);
      };

      const saveResult = (fitting) => {
        if (!savedResults.some(item => item.id === fitting.id)) {
          setSavedResults([...savedResults, fitting]);
        }
      };

      const sortResults = (order) => {
        setSortOrder(order);
        const sorted = [...results].sort((a, b) => {
          if (order === 'asc') return a.price - b.price;
          if (order === 'desc') return b.price - a.price;
          return 0;
        });
        setResults(sorted);
      };

      const handlePriceRangeChange = (e) => {
        const { name, value } = e.target;
        const newValue = value === '' ? '' : Math.max(0, Number(value));
        setPriceRange(prev => ({ ...prev, [name]: newValue }));
      };

      useEffect(() => {
        findBestOptions();
      }, [language, type, specificOption, brand, priceRange]);

      return (
        React.createElement('div', { className: 'container-overlay configurator-container' },
          React.createElement('h1', {
            className: 'text-5xl font-bold text-center mb-10 bg-gradient-to-r from-green-400 to-purple-600 bg-clip-text text-transparent',
            'data-i18n': 'configurator_title'
          }, i18next.t('configurator_title')),
          React.createElement('div', { className: 'flex flex-col md:flex-row justify-center items-center gap-6 mb-10' },
            React.createElement('div', { className: 'w-full md:w-1/4 tooltip' },
              React.createElement('label', { className: 'block text-lg font-medium text-gray-200', 'data-i18n': 'type_label' }, i18next.t('type_label')),
              React.createElement('span', { className: 'tooltip-text', 'data-i18n': 'type_tooltip' }, i18next.t('type_tooltip')),
              React.createElement('select', {
                value: type,
                onChange: (e) => { setType(e.target.value); setSpecificOption(''); },
                className: 'mt-2 block w-full rounded-lg border-gray-600 bg-gray-800 text-white shadow-md focus:ring-green-500 focus:border-green-500 p-3 transition duration-300'
              },
                React.createElement('option', { value: '', 'data-i18n': 'type_placeholder' }, i18next.t('type_placeholder')),
                [...new Set(fittingsDatabase.map(f => f.type))].map(t =>
                  React.createElement('option', { key: t, value: t }, t)
                )
              )
            ),
            React.createElement('div', { className: 'w-full md:w-1/4 tooltip' },
              React.createElement('label', { className: 'block text-lg font-medium text-gray-200', 'data-i18n': 'brand_label' }, i18next.t('brand_label')),
              React.createElement('span', { className: 'tooltip-text', 'data-i18n': 'brand_tooltip' }, i18next.t('brand_tooltip')),
              React.createElement('select', {
                value: brand,
                onChange: (e) => setBrand(e.target.value),
                className: 'mt-2 block w-full rounded-lg border-gray-600 bg-gray-800 text-white shadow-md focus:ring-green-500 focus:border-green-500 p-3 transition duration-300'
              },
                React.createElement('option', { value: '', 'data-i18n': 'brand_placeholder' }, i18next.t('brand_placeholder')),
                ["GTV", "BOYARD", "DECOLINE", "MF"].map(b =>
                  React.createElement('option', { key: b, value: b }, b)
                )
              )
            )
          ),
          React.createElement('div', { className: 'price-inputs-container justify-center' },
            React.createElement('div', { className: 'flex flex-col md:flex-row gap-6' },
              React.createElement('div', { className: 'w-full md:w-1/4' },
                React.createElement('label', { className: 'block text-lg font-medium text-gray-200', 'data-i18n': 'price_range_from' }, i18next.t('price_range_from')),
                React.createElement('input', {
                  type: 'number',
                  name: 'min',
                  value: priceRange.min,
                  onChange: handlePriceRangeChange,
                  className: 'mt-2 block w-full rounded-lg border-gray-600 bg-gray-800 text-white shadow-md focus:ring-green-500 focus:border-green-500 p-3 transition duration-300',
                  min: '0'
                })
              ),
              React.createElement('div', { className: 'w-full md:w-1/4' },
                React.createElement('label', { className: 'block text-lg font-medium text-gray-200', 'data-i18n': 'price_range_to' }, i18next.t('price_range_to')),
                React.createElement('input', {
                  type: 'number',
                  name: 'max',
                  value: priceRange.max,
                  onChange: handlePriceRangeChange,
                  className: 'mt-2 block w-full rounded-lg border-gray-600 bg-gray-800 text-white shadow-md focus:ring-green-500 focus:border-green-500 p-3 transition duration-300',
                  min: '0'
                })
              )
            )
          ),
          type && React.createElement('div', { className: 'mb-8 text-center tooltip' },
            React.createElement('label', { className: 'block text-lg font-medium text-gray-200', 'data-i18n': 'specific_option_label' }, i18next.t('specific_option_label')),
            React.createElement('span', { className: 'tooltip-text', 'data-i18n': 'specific_option_tooltip' }, i18next.t('specific_option_tooltip')),
            React.createElement('select', {
              value: specificOption,
              onChange: (e) => setSpecificOption(e.target.value),
              className: 'mt-2 block w-full md:w-1/3 mx-auto rounded-lg border-gray-600 bg-gray-800 text-white shadow-md focus:ring-green-500 focus:border-green-500 p-3 transition duration-300'
            },
              React.createElement('option', { value: '', 'data-i18n': 'specific_option_placeholder' }, i18next.t('specific_option_placeholder')),
              specificOptions[type].map(option =>
                React.createElement('option', { key: option, value: option }, option)
              )
            )
          ),
          React.createElement('div', { className: 'flex justify-center mb-8 gap-4' },
            React.createElement('button', {
              onClick: findBestOptions,
              className: 'bg-gradient-to-r from-green-500 to-purple-600 text-white px-6 py-3 rounded-xl shadow-lg hover:from-green-600 hover:to-purple-700 transition duration-300 transform hover:scale-105',
              'data-i18n': 'find_options'
            }, i18next.t('find_options')),
            React.createElement('button', {
              onClick: () => sortResults(sortOrder === 'asc' ? 'desc' : 'asc'),
              className: 'bg-gradient-to-r from-green-500 to-purple-600 text-white px-6 py-3 rounded-xl shadow-lg hover:from-green-600 hover:to-purple-700 transition duration-300 transform hover:scale-105',
              'data-i18n': sortOrder === 'asc' ? 'sort_desc' : 'sort_asc'
            }, i18next.t(sortOrder === 'asc' ? 'sort_desc' : 'sort_asc'))
          ),
          error && React.createElement('p', { className: 'text-red-400 text-center mb-4' }, error),
          React.createElement('div', { className: 'tab-content' },
            React.createElement('div', { className: 'tab-buttons flex justify-center mb-6' },
              React.createElement('button', {
                className: `tab-button ${activeTab === 'results' ? 'active' : ''}`,
                onClick: () => setActiveTab('results'),
                'data-i18n': 'tab_results'
              }, i18next.t('tab_results')),
              React.createElement('button', {
                className: `tab-button ${activeTab === 'saved' ? 'active' : ''}`,
                onClick: () => setActiveTab('saved'),
                'data-i18n': 'tab_saved'
              }, i18next.t('tab_saved')),
              React.createElement('button', {
                onClick: () => saveToPDF(false),
                className: 'bg-gradient-to-r from-green-500 to-purple-600 text-white px-4 py-2 rounded-xl shadow-lg hover:from-green-600 hover:to-purple-700 transition duration-300',
                'data-i18n': 'export_selected_pdf'
              }, i18next.t('export_selected_pdf')),
              React.createElement('button', {
                onClick: () => saveToPDF(true),
                className: 'bg-gradient-to-r from-green-500 to-purple-600 text-white px-4 py-2 rounded-xl shadow-lg hover:from-green-600 hover:to-purple-700 transition duration-300',
                'data-i18n': 'export_all_pdf'
              }, i18next.t('export_all_pdf'))
            ),
            activeTab === 'results' && results.length > 0 && React.createElement('div', null,
              React.createElement('h2', {
                className: 'text-4xl font-semibold text-white mb-8 text-center bg-gradient-to-r from-green-400 to-purple-600 bg-clip-text',
                'data-i18n': 'results_title'
              }, i18next.t('results_title')),
              React.createElement('div', { className: 'grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6' },
                results.map(fitting =>
                  React.createElement('div', {
                    key: fitting.id,
                    className: 'bg-gray-800 rounded-xl p-4 shadow-xl hover:bg-gray-700 hover:shadow-2xl transition-all duration-400 transform hover:-translate-y-2'
                  },
                    React.createElement('img', {
                      src: fitting.image,
                      alt: fitting.name,
                      className: 'w-full h-48 object-cover rounded-lg mb-4'
                    }),
                    React.createElement('h3', { className: 'text-xl font-semibold text-green-300' }, `${i18next.t('name_label')}: ${fitting.name}`),
                    React.createElement('p', { className: 'text-gray-300' }, `${i18next.t('type')}: ${fitting.type}`),
                    React.createElement('p', { className: 'text-gray-300' }, `${i18next.t('subtype_label')}: ${fitting.subtype}`),
                    React.createElement('p', { className: 'text-gray-300' }, `${i18next.t('option')}: ${fitting.specificOption}`),
                    React.createElement('p', { className: 'text-gray-300' }, `${i18next.t('brand')}: ${fitting.brand}`),
                    React.createElement('p', { className: 'text-purple-400 font-bold mt-2' }, `${i18next.t('price')}: ${fitting.price} руб.`),
                    React.createElement('p', { className: 'text-gray-400 mt-2' }, `${i18next.t('description')}: ${fitting.description}`),
                    React.createElement('button', {
                      onClick: () => saveResult(fitting),
                      className: 'bg-green-500 text-white px-4 py-2 rounded-lg hover:bg-green-600 mt-4 transition duration-300',
                      'data-i18n': 'save'
                    }, i18next.t('save')),
                    React.createElement('p', { className: 'text-xs text-gray-500 mt-2', 'data-i18n': 'price_disclaimer' }, i18next.t('price_disclaimer'))
                  )
                )
              )
            ),
            activeTab === 'saved' && savedResults.length > 0 && React.createElement('div', null,
              React.createElement('h2', {
                className: 'text-4xl font-semibold text-white mb-8 text-center bg-gradient-to-r from-green-400 to-purple-600 bg-clip-text',
                'data-i18n': 'saved_results_title'
              }, i18next.t('saved_results_title')),
              React.createElement('div', { className: 'grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6' },
                savedResults.map(fitting =>
                  React.createElement('div', {
                    key: fitting.id,
                    className: 'bg-gray-700 rounded-xl p-4 shadow-md hover:bg-gray-600 transition duration-300'
                  },
                    React.createElement('h3', { className: 'text-xl font-semibold text-green-300' }, `${i18next.t('name_label')}: ${fitting.name}`),
                    React.createElement('p', { className: 'text-gray-300' }, `${i18next.t('price')}: ${fitting.price} руб.`),
                    React.createElement('p', { className: 'text-gray-300' }, `${i18next.t('description')}: ${fitting.description}`)
                  )
                )
              )
            )
          )
        )
      );
    };

    const AboutPage = () => (
      React.createElement('div', { className: 'about-container' },
        React.createElement('h2', { className: 'text-4xl font-bold', 'data-i18n': 'about_title' }, i18next.t('about_title')),
        React.createElement('p', { 'data-i18n': 'about_text' }, i18next.t('about_text'))
      )
    );

    const PromotionsPage = () => (
      React.createElement('div', { className: 'container-overlay' },
        React.createElement('h2', { className: 'text-4xl font-bold text-center mb-8 bg-gradient-to-r from-green-400 to-purple-600 bg-clip-text', 'data-i18n': 'promotions_title' }, i18next.t('promotions_title')),
        React.createElement('div', { className: 'promotion-card' },
          React.createElement('p', { 'data-i18n': 'promotion_gtv_discount' }, i18next.t('promotion_gtv_discount'))
        ),
        React.createElement('div', { className: 'promotion-card' },
          React.createElement('p', { 'data-i18n': 'promotion_free_delivery' }, i18next.t('promotion_free_delivery'))
        )
      )
    );

    const WarehousePage = () => (
      React.createElement('div', { className: 'tour-container' },
        React.createElement('h2', { className: 'text-4xl font-bold mb-8 bg-gradient-to-r from-green-400 to-purple-600 bg-clip-text', 'data-i18n': 'warehouse_title' }, i18next.t('warehouse_title')),
        React.createElement('p', { className: 'mb-6', 'data-i18n': 'warehouse_text' }, i18next.t('warehouse_text')),
        React.createElement('iframe', {
          src: 'https://www.youtube.com/embed/dQw4w9WgXcQ',
          title: 'Virtual Tour',
          frameBorder: '0',
          allow: 'accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture',
          allowFullScreen: true
        })
      )
    );

    const ContactsPage = () => (
      React.createElement('div', { className: 'container-overlay' },
        React.createElement('h2', { className: 'text-4xl font-bold text-center mb-8 bg-gradient-to-r from-green-400 to-purple-600 bg-clip-text', 'data-i18n': 'contacts_title' }, i18next.t('contacts_title')),
        React.createElement('p', { className: 'mb-6 text-center', 'data-i18n': 'contacts_text' }, i18next.t('contacts_text'))
      )
    );

    const ReviewsPage = () => (
      React.createElement('div', { className: 'container-overlay' },
        React.createElement('h2', { className: 'text-4xl font-bold text-center mb-8 bg-gradient-to-r from-green-400 to-purple-600 bg-clip-text', 'data-i18n': 'reviews_title' }, i18next.t('reviews_title')),
        React.createElement('p', { className: 'mb-6 text-center', 'data-i18n': 'reviews_text' }, i18next.t('reviews_text'))
      )
    );

    const App = () => {
      const [currentPage, setCurrentPage] = useState('welcome');
      const [language, setLanguage] = useState('ru');

      const renderPage = () => {
        switch (currentPage) {
          case 'welcome': return React.createElement(WelcomePage, { onNavigate: () => setCurrentPage('configurator') });
          case 'configurator': return React.createElement(ConfiguratorPage, { language });
          case 'about': return React.createElement(AboutPage);
          case 'warehouse': return React.createElement(WarehousePage);
          case 'promotions': return React.createElement(PromotionsPage);
          case 'contacts': return React.createElement(ContactsPage);
          case 'reviews': return React.createElement(ReviewsPage);
          default: return React.createElement(WelcomePage, { onNavigate: () => setCurrentPage('configurator') });
        }
      };

      return (
        React.createElement(React.Fragment, null,
          React.createElement(Navbar, { setCurrentPage, onLanguageChange: setLanguage }),
          renderPage()
        )
      );
    };

    const root = createRoot(document.getElementById('root'));
    root.render(React.createElement(App));
  </script>
</body>
</html>
