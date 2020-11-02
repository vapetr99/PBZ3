CREATE
  (USA: Country {name: "USA"}),
  (German: Country {name: "German"}),
  (Japan: Country {name: "Japan"}),

  (Nike: Company {name: "Nike", foundation_date: 1964}) -[:LOCATION]-> (USA),
  (Adidas: Company {name: "Adidas", foundation_date: 1949}) -[:LOCATION]-> (German),
  (Puma: Company {name: "Puma", foundation_date: 1948}) -[:LOCATION]-> (German),
  (Asics: Company {name: "Asics", foundation_date: 1977}) -[:LOCATION]-> (Japan),

  (nfc4: Sneakers {name: "Nike Flex Control 4", price: 190}) -[:SNEAKERS]-> (Nike),
  (naf1sl: Sneakers {name: "Nike Air Force 1 Sage Low", price: 100}) -[:SNEAKERS]-> (Nike),
  (a8c: Sneakers {name: "Adizero 8.0 Cleats", price: 165}) -[:SNEAKERS]-> (Adidas),
  (s500wrs: Sneakers {name: "SPEED 500 Running Shoes", price: 165}) -[:SNEAKERS]-> (Puma),
  (ral: Sneakers {name: "Roma Amor Logo", price: 190}) -[:SNEAKERS]-> (Puma),
  (gr8c: Sneakers {name: "Gel-Resolution 8 Clay", price: 110}) -[:SNEAKERS]-> (Asics),

  (l20md: Slippers {name: "LEADCAT 20 MR DOODLE", price: 90}) -[:SLIPPERS]-> (Puma),
  (pwk: Slippers {name: "PUMA WILO KIDSUPER", price: 87}) -[:SLIPPERS]-> (Puma),
  (adadissage: Slippers {name: "Adidas ADISSAGE", price: 85}) -[:SLIPPERSS]-> (Adidas),
  (adadipure: Slippers {name: "Adidas Adipure", price: 70}) -[:SLIPPERS]-> (Adidas),

  (sport: Hobbie {name: "Sport"}),
  (fashion: Hobbie {name: "Fashion"}),

  (nfc4) -[:APPOINTMENT]-> (sport),
  (naf1sl) -[:APPOINTMENT]-> (fashion),
  (a8c) -[:APPOINTMENT]-> (sport),
  (s500wrs) -[:APPOINTMENT]-> (sport),
  (ral) -[:APPOINTMENT]-> (fashion),
  (gr8c) -[:APPOINTMENT]-> (sport)



// Найти компании, расположенные в Германии
MATCH
  (company: Company) -[:LOCATION]-> (:Country{name: "German"})
RETURN company

// найти кроссовки, произведенные компаниями США
MATCH
  (sneakers: Sneakers) -[:SNEAKERS]-> (company: Company),
  (company) -[:LOCATION]-> (:Country{name: "USA"})
RETURN sneakers

// найти шлепанцы, произведенные в Германии со стоимостью больше 85
MATCH
  (slippers: Slippers) -[:SLIPPERS]-> (company: Company),
  (company) -[:LOCATION]-> (:Country{name: "German"})
  WHERE slippers.price > 85
RETURN slippers

// найти дату оснований компаний, расположенных в Европе
MATCH
  (company: Company) -[:LOCATION]-> (:Country{name: "Japan"})
RETURN company.foundation_date

// Найти количество кроссовок, произведенных в США
MATCH
  (sneakers: Sneakers) -[:SNEAKERS]-> (company: Company),
  (company) -[:LOCATION]-> (:Country{name: "USA"})
RETURN COUNT(sneakers)

// найти среднюю цену шлепанцев, произведенныз в немецких компания, основанных после 1948
MATCH
  (slippers: Slippers) -[:SLIPPERS]-> (company: Company),
  (company) -[:LOCATION]-> (:Country{name: "German"})
  WHERE company.foundation_date > 1948
RETURN AVG(slippers.price)

// найти все пары кроссовок с одной компании
MATCH
  (sneakers: Sneakers) -[:SNEAKERS]-> (company: Company),
  (sneakers2: Sneakers) -[:SNEAKERS]-> (company)
  WHERE sneakers <> sneakers2 AND sneakers.name < sneakers2.name
RETURN sneakers, sneakers2

// найти максимальную стоимость кроссовок, произведенных в Германии и предназначенных для спорта
MATCH
  (sneakers: Sneakers) -[:APPOINTMENT]-> (:Hobbie{name: "Sport"}),
  (sneakers: Sneakers) -[:SNEAKERS]-> (company: Company),
  (company) -[:LOCATION]-> (:Country{name: "German"})
RETURN MAX(sneakers.price)

// найти кроссовки, предназначенные для моды
MATCH
  (sneakers: Sneakers) -[:APPOINTMENT]-> (:Hobbie{name: "Fashion"})
RETURN sneakers

// Немецкие компании, где средняя стоимость шлепанец >= 60
MATCH
  (slippers: Slippers) -[:SLIPPERS]-> (company: Company),
  (company) -[:LOCATION]-> (:Country{name: "German"})
  WITH slippers, company, AVG(slippers.price) AS price
  WHERE price >= 60
RETURN company



