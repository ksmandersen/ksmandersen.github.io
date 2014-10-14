---
layout: post
title: "Generic Data Sources in Swift"
date: 2014-10-14 11:30
published: false
category: code
tags: swift, ios, code
---
{% highlight swift %}
class ArrayDataSource<C: UIView, I: AnyObject>: NSObject, UITableViewDataSource, UICollectionViewDataSource {
	private var items: [I]
	private var reuseIdentifier: String
	private var configureBlock: (C, I) -> Void
	
	init(items: [I], reuseIdentifier: String, configureBlock: (C, I) -> Void) {
		self.items = items
		self.reuseIdentifier = reuseIdentifier
		self.configureBlock = configureBlock
		
		super.init()
	}
	
	func itemAtIndexPath(indexPath: NSIndexPath) -> I? {
		return items[indexPath.row]
	}
	
	func configureCell(cell: C, atIndexPath indexPath: NSIndexPath) {
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
	
	func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
		let cell = tableView.dequeueReusableCellWithIdentifier(reuseIdentifier) as C
		
		configureCell(cell, atIndexPath: indexPath)
		
		return cell as UITableViewCell
	}
	
	// MARK: UICollectionViewDataSource
	
	func numberOfSectionsInCollectionView(collectionView: UICollectionView) -> Int {
		return 1
	}
	
	func collectionView(collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
		return items.count
	}
	
	func collectionView(collectionView: UICollectionView, cellForItemAtIndexPath indexPath: NSIndexPath) -> UICollectionViewCell {
		let cell = collectionView.dequeueReusableCellWithReuseIdentifier(reuseIdentifier, forIndexPath: indexPath) as C
		
		configureCell(cell, atIndexPath: indexPath)
		
		return cell as UICollectionViewCell
	}
}
{% endhighlight %}