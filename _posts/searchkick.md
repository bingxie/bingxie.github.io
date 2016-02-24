custom analyzers

Queries:

result = Product.search "尼康", where: {store_id: 1}

result = Product.search "*", where: {store_id: 1}

, limit: 10, offset: 50

# Search specific fields
fields: [:title, :brand]
