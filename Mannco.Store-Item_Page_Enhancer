// ==UserScript==
// @name         Mannco.Store - Item Page Enhancer
// @namespace    https://github.com/LucasHenriqueDiniz
// @version      0.2
// @description  Mannco Store Enhancer is a browser extension that enhances the Mannco Store website with a range of features to make your shopping experience more efficient and streamlined.
// @author       Lucas Diniz
// @match        *://mannco.store/item/*
// @icon         https://www.google.com/s2/favicons?sz=64&domain=mannco.store
// @grant        GM_getValue
// @grant        GM_setValue
// @grant        GM.xmlHttpRequest
// @require      https://cdnjs.cloudflare.com/ajax/libs/GM_config/1.6.0/gm_config.min.js

// @homepageURL  https://github.com/LucasHenriqueDiniz/Mannco.Store-Item_Page_Enhancer
// @supportURL   https://github.com/LucasHenriqueDiniz/Mannco.Store-Item_Page_Enhancer/issues
// @downloadURL  https://github.com/LucasHenriqueDiniz/Mannco.Store-Item_Page_Enhancer/raw/main/Mannco.Store-Item_Page_Enhancer
// @updateURL    https://github.com/LucasHenriqueDiniz/Mannco.Store-Item_Page_Enhancer/raw/main/Mannco.Store-Item_Page_Enhancer
// ==/UserScript==

(function () {
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //def const DOMS
    const itemPriceBox = document.querySelector("#content > div:nth-child(4) > div > div > div.col-xl-4.col-lg-7 > div.input-group.mb-3 > input");
    const quantityBox = document.querySelector("#content > div:nth-child(4) > div > div > div.col-xl-4.col-lg-7 > div.form-group > input")
    const itemPriceBoxBox = document.querySelector("#content > div:nth-child(4) > div > div > div.col-xl-4.col-lg-7")

    let highestBuyOrder = Number(document.querySelector("#content > div:nth-child(4) > div > div > div.col-xl-8.col-lg-5.mt-md-3 > table > tbody > tr:nth-child(2) > td:nth-child(1)").textContent.slice(1))
    if (isNaN(highestBuyOrder)) {
        highestBuyOrder = 0.00
    }
    const itemPrice = parseFloat(document.querySelector("#content > div.row > div:nth-child(1) > div > div > span.important-text > span").textContent.slice(1))
    const quantidadeBuyOrders = document.querySelector("#content > div:nth-child(4) > div > div > div.col-xl-8.col-lg-5.mt-md-3 > table > tbody").childElementCount - 1
    const moneyAvaible = parseFloat(document.querySelector("#account-dropdown > div > span.account-balance.ecurrency").textContent.replace('$','').trim())

    const parentDiv = document.querySelector("#content > div:nth-child(4) > div > div > div.col-xl-4.col-lg-7 > div.input-group.mb-3")

    const currency = 1 //1 = $ | 2 = £ | 3 Euro | 4 CHLF | 5 py6 | 6 polony | 7 R$
    const itemName = document.querySelector("#page-sidebar > div > div > div.card-item > h2 > span").textContent.trim().replaceAll('#', '%23').replaceAll(`'`, '%27').replaceAll(' ', '%20')
    var appID = window.location.href.match(/(?<=\/)[0-9]{3,6}/g)
    if (appID != 440 && appID != 730 && appID != 252490 && appID != 570) {
        var dt = document.querySelectorAll('dt')
        if (dt[dt.length - 1].textContent === "SKU") {
            appID = 440;
            console.log(appID)
        } else if (dt[dt.length - 2].textContent === "Hero") {
            appID = 570;
            console.log(appID)
        } else if (dt.length === 2) {
            appID = 252490;
            console.log("lenght = 2 - appID = " + appID)
        } else if (dt.length === 4) {
            appID = 730;
            console.log(appID)
        } else {
            console.log("SKU NOT FOUND")
            appID = null;
        }
    }
    var apiLink = `https://steamcommunity.com/market/priceoverview/?appid=${appID}&currency=${currency}&market_hash_name=${itemName}`
    ////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //styles
    var title = 'font-weight: 700;font-size: 1.5rem;color: #fff;margin-bottom: 3px;-webkit-text-stroke-width: thin;'
    var text = 'font-weight: 700;font-size: 1.20rem;color: #fff;'
    //css STYLES
    const cssStyles = `
        @keyframes spinner {
          0% { transform: rotate(0deg); }
          100% { transform: rotate(1080deg); }
        }
        .back-to-top {
            position: fixed;
            bottom: 20px;
            width: 50px;
            height: 50px;
            right: 1%;
            border: none;
            display: none;
            -webkit-text-stroke: thick;
            color: white;
            border-radius: 50%;
            background-color: rgb(50, 51, 81);
            font-size: 20px;
            text-align: center;
            box-shadow: 4px 4px 6px rgba(0, 0, 0, 0.5);
            cursor: pointer;
  	        transform: translateY(0%);
  	        transition: ease-out 250ms;
	        transition-property: transform, background-color, text-decoration, color, display;
        }
	    .back-to-top.show {
	         display: block;
	    }

        .hover-full-button {
	    	transition-property: color,background-color,border-color,fill,stroke,opacity,box-shadow,transform,filter,backdrop-filter,font-size;
		    transition-timing-function: cubic-bezier(.4,0,.2,1);
	    	transition-duration: .5s;

	        margin-top: 5%;
	        font-weight: 700;
	        font-size: 22px;
	        line-height: 25px;
	        box-sizing: border-box;
	        padding-bottom: 0.5rem;
	        padding-left: 1rem;
	        padding-right: 1rem;
            padding-top: 3px;
	        text-align-last: center;
	        background-color: rgba(255, 255, 255, 0);
	        text-align: center;
	        color: rgb(255, 255, 255);
	        outline: white;
	        outline-width: thin;
	        outline-style: auto;
	    	cursor: pointer;
            border-radius: 0.4em;
        }
        .hover-full-button:hover {
          background-color: rgba(255, 255, 255, 1);
          color: black;
          font-size: 23px;
        }
    `;

    const backToTopGoogleIcon = `
    font-size: 45;
    transform: rotate(-90deg);
    text-align: center;
    display: flex;
    justify-content: center;
    `
    const style = document.createElement('style');
    style.appendChild(document.createTextNode(cssStyles));
    document.head.appendChild(style);
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    // Objeto que armazena os valores de configuração
    const settings = GM_getValue('settings', {});
    // Função para criar uma checkbox com valor booleano
    const createCheckbox = (text, name, defaultValue, title, appendElement) => {
        // Cria a checkbox
        const checkbox = document.createElement('input');
        checkbox.type = 'checkbox';
        checkbox.title = title
        checkbox.checked = settings[name] ?? defaultValue;
        checkbox.style.marginRight = '10px';

        // Atualiza o valor da configuração correspondente quando a checkbox é clicada
        checkbox.addEventListener('change', () => {
            settings[name] = checkbox.checked;
            localStorage.setItem('settings', JSON.stringify(settings));
        });

        // Cria a label
        const label = document.createElement('label');
        label.innerText = text;
        label.style.fontSize = "calc(0.5em + 0.5vw)";
        label.title = title
        if (appendElement.children.length === 2) {
            label.style.marginTop = '4rem';
        }
        label.style.marginRight = '10px';
        label.style.marginBottom = '0.5rem';
        label.style.display = 'flex';

        const space = document.createElement('span');
        space.title = title
        space.style.marginRight = '10px'

        label.appendChild(space);
        label.appendChild(checkbox);

        appendElement.appendChild(label);

        // Retorna o valor da configuração correspondente à checkbox
        return checkbox.checked;
    }
    //create a selector using a funtion, define optins before!
    const selector = (text, options, defaultValue, appendElement, title, name) => {
        // Cria a div que contém a label e o select
        const container = document.createElement('div');
        container.style.display = 'flex';
        container.title = title
        container.style.alignItems = 'center';
        container.style.marginBottom = '10px';

        // Cria a label
        const label = document.createElement('div');
        label.innerText = text;
        label.style.fontSize = "calc(0.5em + 0.5vw)";
        label.title = title
        if (appendElement.children.length === 2) {
            label.style.marginTop = '4rem';
        }
        label.style.marginRight = '10px';
        label.style.marginBottom = '0.5rem';
        label.style.display = 'flex';
        container.appendChild(label);

        // Cria o select
        const select = document.createElement('select');
        select.style.marginRight = '10px';
        select.name = name; // Define o nome do select como o nome da configuração

        // Adiciona as opções
        options.forEach((option) => {
            const optionElement = document.createElement('option');
            optionElement.value = option;
            optionElement.text = option;
            select.add(optionElement);
        });

        // Recupera o valor da configuração correspondente do armazenamento local
        const savedValue = localStorage.getItem(name);

        // Seleciona o valor salvo ou o valor padrão
        select.value = savedValue ?? defaultValue;

        // Atualiza o valor da configuração correspondente quando a opção é selecionada
        select.addEventListener('change', () => {
            settings[name] = select.value;
            localStorage.setItem('settings', JSON.stringify(settings)); // Salva o objeto completo no armazenamento local
        });

        // Adiciona o select à div do container
        container.appendChild(select);

        // Adiciona a div do container ao elemento de destino
        appendElement.appendChild(container);

        // Recupera as configurações salvas do armazenamento local
        const savedSettings = JSON.parse(localStorage.getItem('settings'));
        if (savedSettings && savedSettings[name]) {
            // Se a configuração específica já foi salva, usa o valor salvo em vez do valor padrão
            select.value = savedSettings[name];
            settings[name] = savedSettings[name];
        } else {
            // Senão, usa o valor padrão e adiciona a configuração ao objeto
            settings[name] = defaultValue;
        }

        // Retorna o valor da configuração correspondente à opção selecionada
        return select.value;
    };
    //create a valuebox using a fucntion
    const createNumberInput = (text, name, defaultValue, minValue, title, appendElement) => {
        // Cria o input de número
        const numberInput = document.createElement('input');
        numberInput.type = 'number';
        numberInput.title = title;
        numberInput.value = settings[name] ?? defaultValue;
        numberInput.min = minValue;
        numberInput.style.marginRight = '10px';

        // Atualiza o valor da configuração correspondente quando o input é alterado
        numberInput.addEventListener('change', () => {
            settings[name] = Number(numberInput.value);
            localStorage.setItem('settings', JSON.stringify(settings));
        });

        // Cria a label
        const label = document.createElement('label');
        label.innerText = text;
        label.style.fontSize = "calc(0.5em + 0.5vw)";
        label.title = title
        if (appendElement.children.length === 2) {
            label.style.marginTop = '4rem';
        }
        label.style.marginRight = '10px';
        label.style.marginBottom = '0.5rem';
        label.style.display = 'flex';

        const space = document.createElement('span');
        space.title = title;
        space.style.marginRight = '10px';

        label.appendChild(space);
        label.appendChild(numberInput);

        appendElement.appendChild(label);

        // Retorna o valor da configuração correspondente ao input de número
        return Number(numberInput.value);
    }
    // Função para salvar as configurações
    const saveSettings = () => {
        console.log('saving...')
        console.log(GM_setValue('settings', settings))
        GM_setValue('settings', settings);
        Type.alert.success('Configs Updated','')
    }
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //Site dosent a question for iziToast, so i made one
    function showQuestionToast(title, onYes, onNo) {
        iziToast.question({
            timeout: 10000,
            transitionIn: 'bounceInUp',
            transitionOut: 'bounceInDown',
            layout: 1,
            close: false,
            overlay: true,
            displayMode: 'once',
            id: 'question',
            backgroundColor: 'red',
            iconColor: 'white',
            titleSize: '25',
            titleColor: 'white',
            messageSize: '25',
            zindex: 999,
            title: title,
            closeOnEscape: true,
            message: '',
            position: 'center',
            buttons: [
                ['<button><b>YES</b></button>', function (instance, toast) {
                    onYes();
                    instance.hide({ transitionOut: 'fadeOut' }, toast, 'button');
                }, true],
                ['<button>NO</button>', function (instance, toast) {
                    onNo();
                    instance.hide({ transitionOut: 'fadeOut' }, toast, 'button');
                }],
            ],
            onClosing: function (instance, toast, closedBy) {
                console.info('Closing | closedBy: ' + closedBy);
            },
            onClosed: function (instance, toast, closedBy) {
                console.info('Closed | closedBy: ' + closedBy);
            }
        });
    }
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //Buttons FUnctions
    //dunno why
    function RefreshPage() {
        console.log('funcion called')
        window.location.reload();
    }
    // Função para adicionar 1 centavo à maior buy order
    function plusOneCent() {
        quantityBox.value = quantidade;
        itemPriceBox.value = (highestBuyOrder + 0.01).toFixed(2);
        Item.addBuyOrder()
        switch (RefreshAfterButton) {
            case true:
                window.location.reload();
                break;
            case false:
                break;
            default:
                console.log('error with the refreshAfter button', RefreshAfterButton)
        }

    }
    //function to calculate a profit from a buy order
    function calculateFees(x) {
        var valor = 0
        if (typeof x === 'string' && x.indexOf(',') !== -1) {
            valor = parseFloat(x.replace(',','.'))
        } else {
            valor = parseFloat(x)
        }

        if (valor <= 0.00) {
            return parseFloat(0.00)
        } else if (valor === 0.01) {
            return parseFloat(valor).toFixed(2)
        } else if (valor <= 0.10) {
            return parseFloat(valor - 0.01).toFixed(2)
        } else if (valor <= 0.20) {
            return parseFloat(valor - 0.02).toFixed(2)
        } else {
            return parseFloat(valor - (valor * 5/100)).toFixed(2)
        }
    }
    //
    function CheckLucro(BuyOrder) {
        let FixedOrder = parseFloat(BuyOrder).toFixed(2)
        var AfterFees = parseFloat(calculateFees(itemPrice));

        var profit = (AfterFees - FixedOrder).toFixed(2);
        console.log(FixedOrder, AfterFees, profit);
        return parseFloat(profit);
    }
    //function to update the value of the afterFees function
    function updateAfterFees() {
        var value = parseFloat(itemPriceBox.value);
        var afterFees = parseFloat(calculateFees(value)).toFixed(2);
        var profit = parseFloat(CheckLucro(value))
        let quantity = quantityBox.value
        if (quantityBox.value === '') {
            quantity = 1
        }

        var totalProfit = (profit * quantity).toFixed(2)

        // verificação do valor de afterFees
        if (isNaN(afterFees)) {
            afterFeesElement.textContent = "After fees: $NaN";
        } else {
            afterFeesElement.textContent = `After fees: ${afterFees}`
            afterFeesElement.title = `Basically, the fees are calculated as follows: for amounts greater than 0.10, a fee of -0.01 is applied; for amounts greater than 0.20, a fee of -0.02 is applied; for amounts less than 0.20, a fee of -5% is applied`;
        }
        if (isNaN(profit)) {
            ProfitElement.textContent = "Profit: $NaN"
        } else {
            ProfitElement.textContent = `Profit - One: ${profit}, Total: ${totalProfit}`
            ProfitElement.title = `The profit is calculated by \nsubtracting the fees (after fees = ${calculateFees(itemPrice)}) and your buy order (${value}) from the current selling price (${itemPrice}),\n and then multiplying the result by the current quantity (${quantity}). This gives the total profit (${profit}).`;
        }
    }
    //adds a function for just one cent
    function justOneCentFunction() {
        itemPriceBox.value = 0.01
        try {
            quantidade = customQuantityJustOneCent;
        } catch (err) {
            quantidade = 50
            console.log('error with customQuantityJustOneCent, value:', customQuantityJustOneCent)
        }
        switch (ajustWithAvaibleMoney) {
            case true:
                // Verifica se é necessário diminuir a quantidade
                if ((0.01) * quantidade > moneyAvaible) {
                    quantidade = Math.floor(moneyAvaible / (0.01));
                }
                break;
            case false:
                break;
            default:
                console.log('error with ajustWithAvaibleMoney, value:', ajustWithAvaibleMoney)

        }
    }
    //function to a loading circle
    function loadingCircle(element) {

        var loader = document.createElement('div');
        // add the "loader" class to the div element
        loader.classList.add('loader');
        loader.id = 'loadingCircle'

        // set the styles for the loader
        loader.style.margin = 'auto';
        loader.style.border = '5px solid #5ad1e8';
        loader.style.borderRadius = '50%';
        loader.style.borderTop = '5px solid #0f111e';
        loader.style.width = '25px';
        loader.style.height = '25px';
        loader.style.animation = 'spinner 4s linear infinite';
        element.appendChild(loader);
    }
    //Adds google icons to the page
    const link = document.createElement('link');
    link.rel = 'stylesheet';
    link.href = 'https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@48,400,0,0';
    document.head.appendChild(link);
    // function to create a google icon and append to the element
    function googleIcon(iconName,IDname , element, cssStyle) {
        //iconName the name of the icon 'navigate_next, navigate_before'
        //iconSize the size of the icon "INT"
        //element = where you want the icon to be appeneded
        const icon = document.createElement('span');
        icon.style.cssText = cssStyle
        icon.classList.add('icon-asset', 'material-symbols-outlined');
        icon.id = IDname
        icon.textContent = iconName.toLowerCase()
        element.appendChild(icon);
    }

    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    // make the extenion button for configs
    document.querySelector("#content > h2").style.display = 'flex';
    document.querySelector("#content > h2").style.justifyContent = 'space-between';
    let ItemPageConfigsButton = document.createElement('button');
    ItemPageConfigsButton.className = "btn btn-primary ItemPageConfigsButton";
    ItemPageConfigsButton.innerHTML = "Item Page Enhancer Configs";
    ItemPageConfigsButton.setAttribute("style", "margin-top: 15px;background-color: #3958bc; margin-left: 15px; font-size: 1.5rem");
    ItemPageConfigsButton.style = "vertical-align: middle; margin-left: 15px "
    ItemPageConfigsButton.title = "Create a button to make a 1cent buyOrder"
    document.querySelector("#content > h2").appendChild(ItemPageConfigsButton);
    ItemPageConfigsButton.addEventListener('click', () => {
        showOverlay()
    })
    //create a black overlay
    const overlay = document.createElement('div');
    overlay.style.position = 'fixed';
    overlay.style.top = '0';
    overlay.style.left = '0';
    overlay.style.width = '100%';
    overlay.style.height = '100%';
    overlay.style.backgroundColor = 'rgba(0, 0, 0, 0.5)';
    overlay.style.color = 'white';
    overlay.style.zIndex = '9999';
    overlay.style.display = 'none';
    document.body.appendChild(overlay);
    //creates que whiteboard for all the elements be shown
    const whiteboard = document.createElement('div');
    whiteboard.style.cssText = `
    display: inline-flex;
    position: fixed;
    top: 50%;
    left: 50%;
    height: 50%;
    text-align: start;
    font-size: 20px;
    font-weight: 700;
    transform: translate(-50%, -50%);
    padding: 10px;
    background: linear-gradient(45deg, #6197a3, #7db0bd);
    color: white;
    border-radius: 10px;
    align-items: left;
    white-space: nowrap;
    flex-direction: column;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
    overflow-y: scroll;
    flex-wrap: nowrap;
    justify-content: flex-start;
    white-space: nowrap; box-shadow: 0 0 20px rgba(0, 0, 0, 0.5), inset 0 0 20px rgba(255, 255, 255, 0.5);
    margin-top: 20px;
  `;

    overlay.appendChild(whiteboard);
    //Title
    const TitleC = document.createElement('p');
    TitleC.textContent = 'Mannco.Store - Item Page Enhancer';
    TitleC.style.cssText = `
    font-size: 2rem;
    position: absolute;
    top: 10px;
    text-align: center;
    width: 100%;
    font-family: Helvetica, sans-serif;
    left: 50%;
    transform: translateX(-50%);
    background-color: #6197a3;
    color: white;
    border-radius: 5px;
    padding: 5px;
    margin: 0;
  `;

    whiteboard.appendChild(TitleC);
    //clsoe button for the whiteboard
    const closeButton = document.createElement('span');
    closeButton.textContent = '×';
    closeButton.style.position = 'absolute';
    closeButton.style.top = '5px';
    closeButton.style.right = '10px';
    closeButton.style.fontSize = '24px';
    closeButton.style.fontWeight = 'bold';
    closeButton.style.backgroundColor = 'transparent';
    closeButton.style.color = '#333';
    closeButton.style.border = 'none';
    closeButton.style.cursor = 'pointer';
    closeButton.style.outline = 'none';
    whiteboard.appendChild(closeButton);
    closeButton.style.display = 'block'
    closeButton.addEventListener('click', function () {
        hideOverlay();
        saveSettings()
        showQuestionToast(
            'Refresh the page? (recommended)',
            RefreshPage,
            function() {
                console.log('NO clicked');
            }
        );
    });
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //Configs Options
    const Add1CentButton = createCheckbox('• "Boost Order" button: ', '+1cent', true, 'Adds a button to automatically create a buy order for the highest value plus 0.01 cents.', whiteboard);
    const AddJustOneCentButton = createCheckbox('• "Buy for One Cent" button: ', 'JustOneCent', false, '"Adds a button to automatically create a buy order for 0.01 cents.', whiteboard);
    const AddMatchButton = createCheckbox('• "Create Matching Order" button: ', 'MatchBuyOrder', false, '"Adds a button to automatically create a buyorder with the same value as the highest buy order', whiteboard);
    const OrganizeButtons = createCheckbox('• Organize buttons layout: ', 'OrganizeButtons', true, 'Changes the layout for the buttons making better ( in my opnion )', whiteboard); // in future more options nad make a selection box
    const RefreshAfterButton = createCheckbox('• Auto Refresh page after +1cent/JustOneCent/MatchBuyOrder click: ', 'Refresh+1andJust', false,'Automatically refreshes the page after clicking the Boost Order, Buy for One Cent, or Match Buy Order buttons, to ensure that the buy order has been added or updated.', whiteboard);

    const OnKeyPress1Cent = createCheckbox('• Activate +1cent feature on keypress: ', '1CentOnKeyPress', false,'In Development', whiteboard);
    const OnKeyPressJustOneCent = createCheckbox('• Activate JustOneCent feature on keypress: ', 'JustOneCentOnKeyPress', false,'In Development', whiteboard);

    const customQuantity1Cent = createNumberInput('• Custom "Boost Order" BuyOrder quantity: ', 'customQuantity1Cent', '0', '0', 'Enables customization of the quantity for buy orders from the "Boost Order" button. The default values are: 20 for 0.02 or less, 10 for 0.05 or less, 5 for 3.00 or less, and 1 for prices above that. Entering a value will make all buy orders have the same quantity, independent of the item price. ( 0 = default )', whiteboard)
    const customQuantityJustOneCent = createNumberInput('• Custom "Buy for One Cent" BuyOrder quantity: ', 'customQuantityJustOneCent', '50', '0', 'Enables customization of the quantity for buy orders from the "Boost Order" button. The default value is (50) entering a value will make all buy orders have that quantity', whiteboard)
    const customQuantityMatchingOrder = createNumberInput('• Custom "Create Matching Order" BuyOrder quantity: ', 'customQuantityMatchingBuyOrder', '10', '0', 'Enables customization of the quantity for buy orders from the "Create Matching Order" button. The default value is (10) entering a value will make all buy orders have that quantity', whiteboard)
    const ajustWithAvaibleMoney = createCheckbox('• Automatically adjust the buy order quantity based on the available funds in the wallet: ', 'ajustWithAvaibleMoney', true, 'Automatically adjust the buy orders from the buttons based on the available funds in the wallet. For example, if you have $5.00 in your wallet and the buy order is for 10 items at $1.00 each, the order will be adjusted to 5 items.', whiteboard)

    const ChangeBackGroundForNoBuyOrderOptions = ['Red/Green Background', 'Red/Green border', 'none'];
    const ChangeBackGroundForNoBuyOrder = selector("• Changes buy orders background if no matching buy order: ", ChangeBackGroundForNoBuyOrderOptions, "Red/Green Background", whiteboard,'Select between a Red/Green border, Red/Green background, or none. The chosen selection will create a green or red highlight depending on whether you have the highest buy order (green) or not (red).' ,"ChangeBackGroundForNoBuyOrderOptions");

    const chooseCurrencyOptions = ['$ - Dolar', 'Euro', '3', '4', '5', '6', 'R$ - Real']
    const chooseCurrency = selector('• choose the currency for the steamStatus and others', chooseCurrencyOptions, "$- Dolar", whiteboard, 'Choose the currency for the steam price and such')
    const SteamStatus = createCheckbox('• SteamStatus: ', 'SteamStatus', true,'Adds a box showing the values for in steam for the item', whiteboard);
    const ExtraStatus = createCheckbox('• Show Status for more sites?', 'ExtraStatus', false,'In Development', whiteboard);

    const backToTheTopButton = createCheckbox('• Add a button to go to the top of the page: ', 'backToTheTopButton', true, 'add back to the top button', whiteboard)
    const ProfitCalculator = createCheckbox('• Profit Calculator :', 'ProfitCalculator', true,'Displays an estimate of profits based on the buy orders, current sell value, and fees for buy orders made by other users. ( from the buy orders box )', whiteboard);
    const FeesCalculator = createCheckbox('• Show fees value in itemPrice', 'FeesCalcualtor', true,'Adds text showing the fees value and the profit with the current buy order.', whiteboard);
    const smallFixes = createCheckbox('• Small Fixes: ', 'smallFixes', true,'Fix some small things that annoy me', whiteboard);
    const GraphButtonOptions = ['button', 'remove', 'default'];
    const GraphButton = selector('• Button/Remove Sales Graph: ', GraphButtonOptions, "button", whiteboard, "This selection box allows you to customize the display of the sales chart on the website. Choose 'default' to keep it as it is, 'none' to remove the sales chart altogether, or 'button' to create a button that can expand and collapse the chart as needed.");
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //functions for the close button
    function showOverlay() {
        overlay.style.display = 'block';
        whiteboard.style.display = 'flex';
        ;
    }
    function hideOverlay() {
        overlay.style.display = 'none';
        whiteboard.style.display = 'none';
    }
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //Show/Hide Graph Button
    switch (GraphButton) {
        case 'remove':
            document.querySelector("#content > div:nth-child(3)").style.display = 'none'
            break;
        case 'button':
            var header = document.querySelector("#content > div:nth-child(3) > div > h3");
            var content = document.querySelector("#content > div:nth-child(3) > div");
            //Makes default hide
            content.style.maxHeight = "50px";
            content.style.transitionProperty = 'max-height, height'
            content.style.transition = 'ease-out 200ms'
            // Cria o botão
            var salesGraphicBtn = document.createElement('button');
            salesGraphicBtn.textContent = "Show sales graphic";
            salesGraphicBtn.style.position = 'absolute';
            salesGraphicBtn.style.top = '15px';
            salesGraphicBtn.style.right = '20px';

            salesGraphicBtn.className = "btn btn-primary salesGraphicBtn";
            salesGraphicBtn.title = "Show the sales History graphic with the sales details for this item";
            //salesGraphicBtn.classList.add("");
            header.appendChild(salesGraphicBtn);

            // Define o comportamento do botão
            salesGraphicBtn.addEventListener('click', () => {
                if (salesGraphicBtn.dataset.value === "off") {
                    salesGraphicBtn.dataset.value = "on";
                    salesGraphicBtn.textContent = "Show sales graphic";
                    content.style.maxHeight = "50px";
                } else {
                    salesGraphicBtn.dataset.value = "off";
                    salesGraphicBtn.textContent = "Hide sales graphic";
                    content.style.maxHeight = "500px";
                }
            });
            break;
        case 'default':
            console.log('default')
            break;
        default:
            console.log('problem with the GraphButton, value: ', GraphButton)
            break;
    }
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //1+CentButton
    switch (Add1CentButton) {
        case true:
            // Definir quantidade de buy orders de maneira segura
            var quantidade;
            if (customQuantity1Cent === 0) {
                quantidade = (highestBuyOrder < 0.02) ? 20 :
                (highestBuyOrder < 0.10) ? 10 :
                (highestBuyOrder < 1) ? 5 : 1;
            } else {
                quantidade = customQuantity1Cent;
            }
            switch (ajustWithAvaibleMoney) {
                case true:
                    // Verifica se é necessário diminuir a quantidade
                    if ((highestBuyOrder + 0.01) * quantidade > moneyAvaible) {
                        quantidade = Math.floor(moneyAvaible / (highestBuyOrder + 0.01));
                    }
                    break;
                case false:
                    break;
                default:
                    console.log('error with ajustWithAvaibleMoney, value:', ajustWithAvaibleMoney)

            }

            // Cria botão para a ordem de compra de 1 centavo
            var centOrder = document.createElement("button");
            centOrder.id = "AutoOrder";
            centOrder.className = "btn btn-primary AutoOrder";
            centOrder.style.marginTop = '5px';
            centOrder.innerHTML = "Boost Order";
            centOrder.title = 'Create a buy order using the (highest buy order value + 0.01)'
            itemPriceBoxBox.appendChild(centOrder);
            centOrder.addEventListener("click", plusOneCent);
            break;
        case false:
            break;
        default:
            console.log('Problem with the Add1Cent, value:', Add1CentButton)
            break
    }
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //Just One Cent Button
    switch (AddJustOneCentButton) {
        case true:
            var justOneCent = document.createElement("button");
            justOneCent.id = "AutoOrder";
            justOneCent.className = "btn btn-primary AutoOrder";
            justOneCent.style.marginTop = '5px'
            justOneCent.innerHTML = "Buy for One Cent"
            justOneCent.title = "Create a buy order for 0.01";
            itemPriceBoxBox.appendChild(justOneCent);
            justOneCent.addEventListener("click", justOneCentFunction);
            break;
        case false:
            break;
        default:
            console.log('Error with AddJustOneCentButton, value:',AddJustOneCentButton)
    }
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //Create Matching Order
    //switch
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //Organize Buttons
    switch (OrganizeButtons) {
        case true:
            itemPriceBoxBox.childNodes[11].style.marginLeft = '0'
            itemPriceBoxBox.childNodes[11].style.display = 'flex';
            itemPriceBoxBox.childNodes[11].style.flexDirection = 'column';
            document.querySelector("#content > div:nth-child(4) > div > div > div.col-xl-4.col-lg-7 > form > button").style.marginTop = '5px';
            itemPriceBoxBox.style.display = 'flex';
            itemPriceBoxBox.style.flexDirection = 'column';
            break;
        case false:
            console.log('dont organize sadpepe')
            break;
        default:
            console.log('Problem with OrganizeButtons value:', OrganizeButtons)
            break;
    }
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //SteamStatus
    switch (SteamStatus) {
        case true:
            var steamSidebar = document.createElement('div');
            steamSidebar.id = 'steam-sidebar';
            steamSidebar.className = 'card-body steam-sidebar';
            steamSidebar.style.cssText = `
    display: flex;
    flex-direction: column;
    border-radius: 1rem;
    margin-top: 15px;
    background-image: url("https://cdn.icon-icons.com/icons2/2648/PNG/512/steam_square_icon_160782.png");
    background-size: cover;
    border-radius: 0.5rem;
    background-color: rgba(255, 255, 255, 0.5);
    background-position: center;
    outline-style: solid;
    outline-width: thin;
    `;
            document.querySelector('#page-sidebar').appendChild(steamSidebar);
            ////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
            // Create title for card
            const steamtitle = document.createElement('h2');
            steamtitle.className = 'item-name';
            steamtitle.textContent = 'Steam Info';
            steamtitle.style.cssText = 'margin-top: 0.25rem; box-sizing: border-box; font-weight: 700; color: #adaadf; margin-bottom: 15px; text-align: center; color: rgb(255, 255, 255); font-size: 2rem;';
            steamSidebar.appendChild(steamtitle);
            const itemTitles = ['Lower Price', 'Median Price', 'Volume'];
            const itemDesc = ["The lower price value shows the lowest sale price of the current item on Steam at the moment. This value can be useful for buyers looking for the best deal on the item.", "The median price value represents the middle value of all current sales prices for the item on Steam. This value can be a useful reference point for determining the item's general price range and market value.", "The volume value represents the total quantity of the current item being sold on Steam at the moment. This value can give you an idea of the item's popularity and demand."]
            for (let i = 0; i < itemTitles.length; i++) {
                const dl = document.createElement('div');
                dl.style.cssText = `
                     margin-top: 5px;
                     align-items: center;
                     display: flex;
                     flex-direction: column;
                     `;
                dl.className = 'dl';
                steamSidebar.appendChild(dl);

                const txt = document.createElement('div');
                const value = document.createElement('div');
                txt.innerHTML = itemTitles[i];
                txt.style = title;
                txt.title = itemDesc[i]
                dl.appendChild(txt);

                value.innerHTML = '';
                value.id = 'sidebar-text'
                value.title = itemDesc[i]
                value.style = text;
                dl.appendChild(value);
                loadingCircle(value);
            }
            var openOnSteamButton = document.createElement('button')
            openOnSteamButton.textContent = 'Open on steam'
            openOnSteamButton.className = 'hover-full-button'
            openOnSteamButton.addEventListener("click", function() {
                Type.alert.success(`a window for ${itemName.replaceAll('%23', '#').replaceAll('%27', `'`).replaceAll('%20', ' ')} was open`,'')
                var SteamSearchLink = `https://steamcommunity.com/market/listings/${appID}/${itemName}`;
                window.open(SteamSearchLink)
            });
            steamSidebar.appendChild(openOnSteamButton)
            ////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
            //request the steam item
            GM.xmlHttpRequest({
                method: "GET",
                url: apiLink,
                responseType: "json",
                onload: function (response) {
                    var obj = JSON.parse(response.responseText);
                    console.log(obj, obj.lowest_price, obj.median_price, obj.volume)
                    if (obj.lowest_price === undefined) {
                        document.querySelectorAll('#sidebar-text')[0].textContent = "❌ Error retrieving value ❌"
                        document.querySelectorAll('#sidebar-text')[0].style.fontSize = '1.1rem'
                    } else {
                        document.querySelectorAll('#sidebar-text')[0].textContent = obj.lowest_price
                    }

                    if (obj.median_price === undefined) {
                        document.querySelectorAll('#sidebar-text')[1].textContent = "❌ Error retrieving value ❌"
                        document.querySelectorAll('#sidebar-text')[1].style.fontSize = '1.1rem';
                    } else {
                        document.querySelectorAll('#sidebar-text')[1].textContent = obj.median_price
                    }

                    if (obj.volume === undefined) {
                        document.querySelectorAll('#sidebar-text')[2].textContent = "❌ Error retrieving value ❌"
                        document.querySelectorAll('#sidebar-text')[2].style.fontSize = '1.1rem'
                    } else {
                        document.querySelectorAll('#sidebar-text')[2].textContent = obj.volume
                    }
                }
            })
            break;
        case false:
            break;
        default:
            console.log('problem with steam status var, value: ', SteamStatus)
    }
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //ExtraStatus
    //switch
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //onKeyPress exec 1+ cent
    //switch
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //Red/Green Border/NoBorder None background for box
    //switch
    switch (ChangeBackGroundForNoBuyOrder) {
        case 'Red/Green Background':
            if (itemPriceBox.value === highestBuyOrder) {
                itemPriceBoxBox.parentElement.parentElement.style.backgroundColor = 'green'
            } else {
                itemPriceBoxBox.parentElement.parentElement.style.backgroundColor = 'red'
            }
            break;
        case 'Red/Green border':
            if (itemPriceBox.value === highestBuyOrder) {
                itemPriceBoxBox.parentElement.parentElement.style.borderStyle = 'inset';
                itemPriceBoxBox.parentElement.parentElement.style.borderRadius = 'inherit';
                itemPriceBoxBox.parentElement.parentElement.style.borderColor = 'green';
            } else {
                itemPriceBoxBox.parentElement.parentElement.style.borderStyle = 'inset';
                itemPriceBoxBox.parentElement.parentElement.style.borderRadius = 'inherit';
                itemPriceBoxBox.parentElement.parentElement.style.borderColor = 'red';
            }
            break;
        case 'none':
            break;
        default:
            console.log('error in ChangeBackGroundForNoBuyOrder', ChangeBackGroundForNoBuyOrder)
    }
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //Profit Calculator
    switch (ProfitCalculator) {
        case true:
            var ProfitLabel = document.createElement('th')
            ProfitLabel.scope = 'col'
            ProfitLabel.title = 'The profit is calculated by [(fees - BuyValue) - SellValue]'
            ProfitLabel.textContent = "Profit"
            document.querySelector("#content > div:nth-child(4) > div > div > div.col-xl-8.col-lg-5.mt-md-3 > table > tbody > tr:nth-child(1)").appendChild(ProfitLabel)
            for (var i = 1; quantidadeBuyOrders >= i; i++) {
                let currentBuyOrderRaw = document.querySelector(`#content > div:nth-child(4) > div > div > div.col-xl-8.col-lg-5.mt-md-3 > table > tbody > tr:nth-child(${i + 1}) > td:nth-child(1)`);
                let currentBuyOrder = currentBuyOrderRaw.textContent.slice(1).replace(' or less', '');

                currentBuyOrderRaw.parentElement.addEventListener('click', () => {
                    itemPriceBox.value = currentBuyOrder;
                    try {
                        updateAfterFees()
                    } catch(err) {
                        console.log(updateAfterFees())
                        console.log(err)
                    }
                });
                currentBuyOrderRaw.parentNode.addEventListener('mouseover', () => {
                    currentBuyOrderRaw.parentNode.style.backgroundColor = 'rgba(255, 255, 255, 0.3)';
                    currentBuyOrderRaw.parentNode.style.transition = 'background-color 0.2s ease 0s';
                });

                currentBuyOrderRaw.parentNode.addEventListener('mouseout', () => {
                    currentBuyOrderRaw.parentNode.style.backgroundColor = 'rgba(0, 0, 0, 0)';
                    currentBuyOrderRaw.parentNode.style.transition = 'background-color 0.2s ease 0s';
                });


                var NewElement = document.createElement('td')
                NewElement.textContent = ("$" + CheckLucro(currentBuyOrder))
                document.querySelector("#content > div:nth-child(4) > div > div > div.col-xl-8.col-lg-5.mt-md-3 > table > tbody").children[i].appendChild(NewElement)
                try {
                    if (i % 2 != 0) {
                        currentBuyOrder.style.backgroundColor = 'rgba(0, 0, 0, 0.5)'
                    }
                } catch (TypeError) {
                }
            }
            break;
        case false:
            break;
        default:
            console.log('some error with the profit calculator, value: ', ProfitCalculator)
    }
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //AfterFees and PRofit with current BUy Order
    switch (FeesCalculator) {
        case true:
            var afterFeesElement = document.createElement("div");
            afterFeesElement.textContent = "After fees: $NaN"; // definindo o conteúdo inicial
            afterFeesElement.style.color = "gray";
            afterFeesElement.style.position = "absolute";
            afterFeesElement.style.bottom = "0";
            afterFeesElement.style.left = "0";
            afterFeesElement.style.width = "100%";
            afterFeesElement.style.textAlign = "center";
            afterFeesElement.style.paddingBottom = '5px';
            itemPriceBox.parentNode.appendChild(afterFeesElement);
            afterFeesElement.style.height = (afterFeesElement.offsetHeight * 2 + 'px')

            var ProfitElement = document.createElement("div");
            ProfitElement.textContent = "Profit: $NaN"; // definindo o conteúdo inicial
            ProfitElement.style.color = "gray";
            ProfitElement.style.position = "absolute";
            ProfitElement.style.bottom = "0";
            ProfitElement.style.left = "0";
            ProfitElement.style.width = "100%";
            ProfitElement.style.textAlign = "center";
            ProfitElement.style.paddingBottom = '5px';
            itemPriceBox.parentNode.appendChild(ProfitElement);

            // small fixes
            itemPriceBoxBox.childNodes[1].style.marginBottom = '25px';
            itemPriceBoxBox.childNodes[1].style.fontSize = '1.5rem'
            itemPriceBox.style.backgroundColor = 'rgba(0, 0, 0, 0.3)';
            itemPriceBox.style.borderRadius = '1rem 0 1rem 0'
            //add update function to itemPricebox
            itemPriceBox.addEventListener("input", updateAfterFees);
            itemPriceBox.addEventListener("change", updateAfterFees);

            var parentHeight = itemPriceBox.offsetHeight + afterFeesElement.offsetHeight + 10;
            parentDiv.style.height = parentHeight + "px";
            parentDiv.style.backgroundColor = '#202334'
            parentDiv.style.borderRadius = '1rem';

            document.querySelector("#recipient-username-addon").style.borderBottomRightRadius = '2rem'
            document.querySelector("#recipient-username-addon").style.borderTopRightRadius = '1rem'
            break;
        case false:
            break;
        default:
            console.log('error in the fees calculator')
    }
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //small fixes
    switch (smallFixes) {
        case true:
            document.querySelector("#header-navbar > ul.navbar-nav.header__navbar-links > li:nth-child(1)").remove()
            document.querySelector("#content > div:nth-child(4) > div > div > div.col-xl-4.col-lg-7 > p").style.display = 'none'
            itemPriceBoxBox.parentElement.parentElement.style.borderRadius = '1rem'
            itemPriceBoxBox.childNodes[1].style.marginBottom = '10px';
            itemPriceBoxBox.childNodes[1].style.fontSize = '1.5rem'
            var cardBodies = document.querySelectorAll('.card-body');
            cardBodies.forEach(cardBody => {
                cardBody.style.boxShadow = '3 3 15px rgba(0, 0, 0, 0.3)';
            });
            break;
        case false:
            break;
        default:
            console.log('Problem with smallFixes, value:', smallFixes)
            break
    }
    //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //options ask if want to RefreshPage() after the +1cent and JustOneCent
    // in the plusOneCent and JustOneCent functions
    /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    // Back to the top button
    switch (backToTheTopButton) {
        case true:
            var backToTopButton = document.createElement("button");
            backToTopButton.innerHTML = "";
            googleIcon('navigate_next', 'navigateNext', backToTopButton, backToTopGoogleIcon)
            backToTopButton.title = 'go back to the top of the page'
            backToTopButton.classList.add("back-to-top");
            document.body.appendChild(backToTopButton);

            // had a hardtime working with cssstyles so i decided just implemente everything using js fuckit
            backToTopButton.addEventListener("click", function() {
                window.scrollTo({
                    top: 0,
                    behavior: "smooth"
                });
            });
            backToTopButton.addEventListener('mouseover', () => {
                backToTopButton.style.backgroundColor = 'rgb(90 209 232)'
                backToTopButton.style.color = 'black'
                backToTopButton.style.borderTopStyle = 'outset'
                backToTopButton.style.borderTopColor = 'rgba(255,255,255,0.3)'
                backToTopButton.style.borderTopWidth = 'medium'
                backToTopButton.style.borderBottomStyle = 'outset'
                backToTopButton.style.borderBottomColor = 'rgba(0,0,0,0.5)'
                backToTopButton.style.borderBottomWidht = 'medium'
                backToTopButton.style.boxShadow = '4px 4px 10px rgba(255, 255, 255, 0.3)'
                backToTopButton.style.transform = "translateY(-22%)";

            });
            backToTopButton.addEventListener('mouseout', () => {
                backToTopButton.style.boxShadow = '4px 4px 10px rgba(0, 0, 0, 0.5)'
                backToTopButton.style.backgroundColor = 'rgb(50, 51, 81)'
                backToTopButton.style.border = 'none'
                backToTopButton.style.color = 'white'
                backToTopButton.style.transform = "translateY(0%)";
            });
            window.addEventListener("scroll", function() {
                // Calcula a posição atual da rolagem em relação à altura total da página
                const scrollPosition = window.scrollY;
                const totalHeight = document.body.clientHeight - window.innerHeight;
                const scrollPercentage = scrollPosition / totalHeight * 100;

                // Atualiza a classe do botão com base na posição atual da rolagem
                if (scrollPercentage > 75) {
                    backToTopButton.classList.add("show");
                } else {
                    backToTopButton.classList.remove("show");
                }
            });
            break;
        case false:
            break;
        default:
            console.log('problem with back to the top, value: ', backToTheTopButton)
    }
    /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    //Header beautify
    //
    /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

})();
