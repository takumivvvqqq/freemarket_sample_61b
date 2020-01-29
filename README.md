## prefecturesテーブル
|Column|Type|Options|
|------|----|-------|
|name|string|null: false, unique: true|

### Association
- has_many :address
- has_many :items

## usersテーブル
|Column|Type|Options|
|------|----|-------|
|nickname|string|null: false, index: true|
|first_name|string|null: false|
|last_name|string|null: false|
|first_name_kana|string|null: false|
|last_name_kana|string|null: false|
|email|string|null: false, unique: true|
|password|string|null: false|
|year|integer|null: false|
|month|integer|null: false|
|day|integer|null: false|

### Association
- has_one :address
- has_many :sell_items, class_name: 'Item', :foreign_key => 'sell_id'
- has_many :buy_items, class_name: 'Item', dependent: :destroy, :foreign_key => 'buyer_id'
- has_many :comments, dependent: :destroy
- has_many :likes, dependent: :destroy

## addressテーブル
|Column|Type|Options|
|------|----|-------|
|zip_code|string|null: false, unique: true|
|prefecture_id|references|null: false, foreign_key: true|
|city|string|null: false|
|address1|string|null: false, unique: true|
|address2|string||
|user_id|references|null: false|

### Association
- belongs_to :user
- belongs_to :prefecture

## likesテーブル
|Column|Type|Options|
|------|----|-------|
|user_id|references|null: false, foreign_key: true|
|item_id|references|null: false, foreign_key: true|

### Association
- belongs_to :user
- belongs_to :item

## ratingsテーブル
|Column|Type|Options|
|------|----|-------|
|buyer_id|references|null: false, foreign_key: true|
|seller_id|references|null: false, foreign_key: true|
|rate|string|null: false, default: "good"|

### Association
- belongs_to :buyer, class_name: 'User', :foreign_key => 'buyer_id'
- belongs_to :seller, class_name: 'User', :foreign_key => 'seller_id'

## itemsテーブル
|Column|Type|Options|
|------|----|-------|
|name|string|null: false, index: true|
|price|integer|null: false|
|postage|boolean|null: false|
|description|text|null: false|
|prefecture_id|references|null: false, foreign_key: true|
|seller_id|references|null: false, foreign_key: true|
|buyer_id|references|foreign_key: true|
|brand_id|references|null: false, foreign_key: true|
|size_id|references|null: false, foreign_key: true|

### Association
- belongs_to :prefecture
- belongs_to :seller, class_name: "User"
- belongs_to :buyer, class_name: "User"
- belongs_to :brand
- belongs_to :size
- has_many :comments, dependent: :destroy
- has_many :images, dependent: :destroy
- has_many :likes, dependent: :destroy
- has_many :item_categories
- has_many :categories, through: :item_categories

## sizesテーブル
|Column|Type|Options|
|------|----|-------|
|size|string|null: false|
|category_id|references|null: false, foreign_key: true|

### Association
- belongs_to :category
- has_many :items

## imagesテーブル
|Column|Type|Options|
|------|----|-------|
|image_url|string||
|item_id|references|null: false, foreign_key: true|

### Association
- belongs_to :item

## commentsテーブル
|Column|Type|Options|
|------|----|-------|
|text|text|null: false|
|user_id|references|null: false, foreign_key: true|
|item_id|references|null: false, foreign_key: true|

### Association
- belongs_to :item
- belongs_to :user

## brandsテーブル
|Column|Type|Options|
|------|----|-------|
|name|string|null: false|

### Association
- has_many :items
- has_many :brand_categories
- has_many :categories, through: :brand_categories

## categoriesテーブル
|Column|Type|Options|
|------|----|-------|
|name|string|null: false|
|parent_id|references|foreign_key: true|

### Association
- has_many :children, class_name 'Categories', foreign_key: 'parent_id', dependent: :destroy
- belongs_to :parent, clss_name: 'Categories', optional: true
- has_many :sizes
- has_many :item_categories
- has_many :items, through: :item_categories
- has_many :brand_categories
- has_many :brands, through: :brand_categories

## brand_categoriesテーブル
|Column|Type|Options|
|------|----|-------|
|brand_id|references|null: false, foreign_key: true|
|category_id|references|null: false, foreign_key: true|

### Association
- belongs_to :brand
- belongs_to :category

## item_categoriesテーブル
|Column|Type|Options|
|------|----|-------|
|item_id|references|null: false, foreign_key: true|
|category_id|references|null: false, foreign_key: true|

### Association
- belongs_to :item
- belongs_to :category