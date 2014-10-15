---
layout: post
title: "Generic Data Sources in Swift"
date: 2014-10-14 11:30
published: true
category: code
tags: swift, ios, code
---

In my endless pursuit of defeating the Massive View Controller I have been separating
the data sources of TableViews and CollectionViews into seperate classes. This is a practice
I picked up long ago when [the first issue](http://www.objc.io/issue-1/) of [objc.io](http://www.objc.io) came out.
In that issue [Florian Kugler](http://twitter.com/floriankugler) describes how to write [clan table view code](http://www.objc.io/issue-1/table-views.html) by separating the data source of your table views from the view controller.

{% highlight objective-c %}
void (^configureCell)(PhotoCell*, Photo*) = ^(PhotoCell* cell, Photo* photo) {
   cell.label.text = photo.name;
};
photosArrayDataSource = [[ArrayDataSource alloc] initWithItems:photos
                                                cellIdentifier:PhotoCellIdentifier
                                            configureCellBlock:configureCell];
self.tableView.dataSource = photosArrayDataSource;
{% endhighlight %}

With the introduction of Swift this year I felt the need to re-evaluate and generalize the extraction of data sources
further. 

{% highlight swift %}
class TrackedViewController: UITableViewController {
	private var dataSource: RealmDataSource<TrackedCell, RealmShow>!
	
	override func viewDidLoad() {
		super.viewDidLoad()
		
		let trackedShows = controller.realmStore.trackedShows()
		var block: (TrackedCell, RealmShow) -> Void
		block = { cell, show in
			if let textLabel = cell.textLabel {
				textLabel.text = show.title
			}
		}
		
		dataSource = RealmDataSource<TrackedCell, RealmShow>(
			realmObjects: trackedShows,
			reuseIdentifier: "trackedShowCell",
			configureBlock: block)
	}
}
{% endhighlight %}

{% highlight swift %}
class ArrayDataSource<CellType: UIView, ItemType: AnyObject>: NSObject, 
                                                UITableViewDataSource, 
                                                UICollectionViewDataSource {

	private var items: [ItemType]
	private var reuseIdentifier: String
	private var configureBlock: (CellType, ItemType) -> Void
	
	init(items: [ItemType], 
		 reuseIdentifier: String, 
		 configureBlock: (CellType, ItemType) -> Void) {

		self.items = items
		self.reuseIdentifier = reuseIdentifier
		self.configureBlock = configureBlock
		
		super.init()
	}
	
	func itemAtIndexPath(indexPath: NSIndexPath) -> ItemType? {
		return items[indexPath.row]
	}
	
	func configureCell(cell: CellType, 
					   atIndexPath indexPath: NSIndexPath) {

		if let item = itemAtIndexPath(indexPath) {
			configureBlock(cell, item)
		}
	}
	
	// MARK: UITableViewDataSource
	
	func numberOfSectionsInTableView(tableView: UITableView) -> Int {
		return 1
	}
	
	func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
		return items.count
	}
	
	func tableView(tableView: UITableView, 
				   cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {

		let cell = tableView.dequeueReusableCellWithIdentifier(reuseIdentifier) as CellType
		
		configureCell(cell, atIndexPath: indexPath)
		
		return cell as UITableViewCell
	}
	
	// MARK: UICollectionViewDataSource
	
	func numberOfSectionsInCollectionView(collectionView: UICollectionView) -> Int {
		return 1
	}
	
	func collectionView(collectionView: UICollectionView, 
						numberOfItemsInSection section: Int) -> Int {
		return items.count
	}
	
	func collectionView(collectionView: UICollectionView, 
						cellForItemAtIndexPath indexPath: NSIndexPath) -> UICollectionViewCell {

		let cell = collectionView.dequeueReusableCellWithReuseIdentifier(reuseIdentifier, 
																		 forIndexPath: indexPath) as CellType
		
		configureCell(cell, atIndexPath: indexPath)
		
		return cell as UICollectionViewCell
	}
}
{% endhighlight %}