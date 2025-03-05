<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Países Visitados</title>
    <style>
        body {
            font-family: 'Courier New', Courier, monospace;
            margin: 0;
            padding: 20px;
            background-image: url('https://st.depositphotos.com/1484771/4898/v/950/depositphotos_48983851-stock-illustration-background-with-vintage-stamps.jpg');
            background-size: cover;
            background-attachment: fixed;
            font-weight: bold;
            color: #fff;
        }

        h1 {
            text-align: center;
            color: #333;
            background-color: rgba(255, 255, 255, 0.7);
            padding: 10px;
            border-radius: 5px;
            font-weight: bold;
            font-family: 'Courier New', Courier, monospace;
            margin: 20px auto;
            max-width: 80%;
        }

        h2 {
            margin-top: 30px;
            padding: 15px;
            border-radius: 15px;
            text-align: center;
            font-weight: bold;
            background-color: rgba(255, 255, 255, 1);
            font-family: 'Courier New', Courier, monospace;
            margin: 20px auto;
            max-width: 80%;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }

        /* Colores personalizados para los continentes */
        #africa {
            color: #3ccee2; /* Azul */
            border: 3px solid #3ccee2; /* Azul borde */
        }

        #america {
            color: #ee1a1a; /* Rojo */
            border: 3px solid #ee1a1a; /* Rojo borde */
        }

        #europa {
            color: #2eae3f; /* Verde oscuro */
            border: 3px solid #2eae3f; /* Verde borde */
        }

        #asia {
            color: #712eae; /* Morado */
            border: 3px solid #712eae; /* Morado borde */
        }

        #oceania {
            color: #eddb33; /* Dorado */
            border: 3px solid #eddb33; /* Dorado borde */
        }

        .country-list {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
        }

        label {
            display: flex;
            align-items: center;
            background-color: rgba(0, 0, 0, 0.7);
            padding: 10px;
            border-radius: 5px;
            transition: background-color 0.3s ease;
        }

        label:hover {
            background-color: #bdc3c7;
        }

        input[type="checkbox"] {
            margin-right: 10px;
        }

        .continent-container {
            margin-bottom: 30px;
            padding: 20px;
            border-radius: 15px;
        }

        /* Estilos para el contador */
        #count {
            text-align: center;
            font-size: 1.5em;
            margin-top: 20px;
            color: #fff;
            font-weight: bold;
            background-color: rgba(0, 0, 0, 0.6);
            padding: 10px;
            border-radius: 10px;
        }
    </style>
    <script>
        document.addEventListener("DOMContentLoaded", function () {
            const continents = {
                "África": ["Argelia", "Angola", "Benín", "Botsuana", "Burkina Faso", "Burundi", "Cabo Verde", "Camerún", "Chad", "Comoras", "Costa de Marfil", "Egipto", "Eritrea", "Esuatini", "Etiopía", "Gabón", "Gambia", "Ghana", "Guinea", "Guinea-Bisáu", "Guinea Ecuatorial", "Kenia", "Lesoto", "Liberia", "Libia", "Madagascar", "Malaui", "Mali", "Marruecos", "Mauricio", "Mauritania", "Mozambique", "Namibia", "Níger", "Nigeria", "República Centroafricana", "República del Congo", "República Democrática del Congo", "Ruanda", "Santo Tomé y Príncipe", "Senegal", "Seychelles", "Sierra Leona", "Somalia", "Sudáfrica", "Sudán", "Sudán del Sur", "Tanzania", "Togo", "Túnez", "Uganda", "Zambia", "Zimbabue"],
                "América": ["Antigua y Barbuda", "Argentina", "Bahamas", "Barbados", "Belice", "Bolivia", "Brasil", "Canadá", "Chile", "Colombia", "Costa Rica", "Cuba", "Dominica", "Ecuador", "El Salvador", "Estados Unidos", "Granada", "Guatemala", "Guyana", "Haití", "Honduras", "Jamaica", "México", "Nicaragua", "Panamá", "Paraguay", "Perú", "República Dominicana", "San Cristóbal y Nieves", "San Vicente y las Granadinas", "Santa Lucía", "Surinam", "Trinidad y Tobago", "Uruguay", "Venezuela"],
                "Asia": ["Afganistán", "Arabia Saudita", "Armenia", "Azerbaiyán", "Bangladés", "Baréin", "Birmania (Myanmar)", "Brunéi", "Bután", "Camboya", "Catar", "China", "Corea del Norte", "Corea del Sur", "Emiratos Árabes Unidos", "Filipinas", "Georgia", "India", "Indonesia", "Irak", "Irán", "Israel", "Japón", "Jordania", "Kazajistán", "Kuwait", "Kirguistán", "Laos", "Líbano", "Malasia", "Maldivas", "Mongolia", "Nepal", "Omán", "Pakistán", "Singapur", "Siria", "Sri Lanka", "Tailandia", "Tayikistán", "Timor Oriental", "Turkmenistán", "Turquía", "Uzbekistán", "Vietnam", "Yemen"],
                "Europa": ["Albania", "Alemania", "Andorra", "Austria", "Bélgica", "Bielorrusia", "Bosnia y Herzegovina", "Bulgaria", 
                    "República Checa", "Chipre", "Croacia", "Dinamarca", "Eslovaquia", "Eslovenia", "España", "Estonia", 
                    "Finlandia", "Francia", "Reino Unido", "Grecia", "Países Bajos", "Hungría", "Italia", "Irlanda", 
                    "Islandia", "Letonia", "Liechtenstein", "Lituania", "Luxemburgo", "Macedonia del Norte", "Moldavia", 
                    "Malta", "Mónaco", "Noruega", "Polonia", "Portugal", "Rumania", "Rusia", "San Marino", 
                    "Serbia", "Suecia", "Suiza", "Ucrania"
                ],
                "Oceanía": ["Australia", "Fiyi", "Islas Marshall", "Islas Salomón", "Kiribati", "Micronesia", "Nauru", "Nueva Zelanda", "Palaos", "Papúa Nueva Guinea", "Samoa", "Tonga", "Tuvalu", "Vanuatu"]
            };

            const container = document.getElementById("country-list");
            const countDisplay = document.getElementById("count");
            let storedCountries = JSON.parse(localStorage.getItem("visitedCountries")) || [];

            // Función para limpiar y normalizar los nombres de los continentes para usarlos como IDs
            function normalizeId(id) {
                return id
                    .toLowerCase()
                    .normalize("NFD")
                    .replace(/[\u0300-\u036f]/g, "")
                    .replace(/[^a-z0-9]/g, "");
            }

            // Función para actualizar el contador
            function updateCountryCount() {
                const visitedCount = storedCountries.length;
                const totalCount = Object.values(continents).reduce((acc, countries) => acc + countries.length, 0);
                countDisplay.textContent = `Países visitados: ${visitedCount} de ${totalCount}`;
            }

            Object.keys(continents).forEach(continent => {
                const continentContainer = document.createElement("div");
                continentContainer.className = "continent-container";

                const title = document.createElement("h2");
                title.textContent = continent;
                title.id = normalizeId(continent);
                continentContainer.appendChild(title);

                const grid = document.createElement("div");
                grid.className = "country-list";

                continents[continent].forEach(country => {
                    const label = document.createElement("label");
                    const checkbox = document.createElement("input");
                    checkbox.type = "checkbox";
                    checkbox.value = country;
                    checkbox.checked = storedCountries.includes(country);

                    checkbox.addEventListener("change", () => {
                        // Actualizar lista de países visitados
                        if (checkbox.checked) {
                            if (!storedCountries.includes(country)) {
                                storedCountries.push(country);
                            }
                        } else {
                            storedCountries = storedCountries.filter(c => c !== country);
                        }
                        localStorage.setItem("visitedCountries", JSON.stringify(storedCountries));
                        updateCountryCount();  // Actualizar el contador
                    });

                    label.appendChild(checkbox);
                    label.appendChild(document.createTextNode(country));
                    grid.appendChild(label);
                });

                continentContainer.appendChild(grid);
                container.appendChild(continentContainer);
            });

            // Inicializar el contador al cargar la página
            updateCountryCount();
        });
    </script>
</head>
<body>
    <h1>¿Cuántos lugares has descubierto?</h1>
    <div id="country-list"></div>
    <div id="count">Países visitados: 0</div>  <!-- Sección para mostrar el recuento -->
</body>
</html>

