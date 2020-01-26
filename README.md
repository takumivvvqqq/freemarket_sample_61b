## areasテーブル
|Column|Type|Options|
|------|----|-------|
|name|string|null: false, unique: true|

### Association
- has_many :users
- has_many :items

## usersテーブル
|Column|Type|Options|
|------|----|-------|
|nickname|string|null: false, index: true|
|email|string|null: false, unique: true|
|password|string|null: false|
|image|string||
|area_id|references|null: false, foreign_key: true|
|address|string|null: false|

### Association
- belongs_to :area
- has_many :items, dependent: :destroy
- has_many :comments, dependent: :destroy
- has_many :likes, dependent: :destroy

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
|area_id|references|null: false, foreign_key: true|
|seller_id|references|null: false, foreign_key: true|
|buyer_id|references|foreign_key: true|
|brand_id|references|null: false, foreign_key: true|
|size_id|references|null: false, foreign_key: true|

### Association
- belongs_to :area
- belongs_to :user
- belongs_to :brand
- belongs_to :size
- has_many :comments, dependent: :destroy
- has_many :images, dependent: :destroy
- has_many :likes, dependent: :destroy
- has_many :item_categorys
- has_many :categorys, through: :item_categorys

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
- has_many :brand_categorys
- has_many :categorys, through: :brand_categorys

## categorysテーブル
|Column|Type|Options|
|------|----|-------|
|name|string|null: false|
|parent_id|references|foreign_key: true|

### Association
- has_many :children, class_name 'Categorys', foreign_key: 'parent_id', dependent: :destroy
- belongs_to :parent, clss_name: 'Categorys', optional: true
- has_many :sizes
- has_many :item_categorys
- has_many :items, through: :item_categorys
- has_many :brand_categorys
- has_many :brands, through: :brand_categorys

## brand_categorysテーブル
|Column|Type|Options|
|------|----|-------|
|brand_id|references|null: false, foreign_key: true|
|category_id|references|null: false, foreign_key: true|

### Association
- belongs_to :brand
- belongs_to :category

## item_categorysテーブル
|Column|Type|Options|
|------|----|-------|
|item_id|references|null: false, foreign_key: true|
|category_id|references|null: false, foreign_key: true|

### Association
- belongs_to :item
- belongs_to :category
