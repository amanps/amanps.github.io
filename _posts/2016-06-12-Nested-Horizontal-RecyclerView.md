---
layout: post
title: Nested, horizontally scrolling RecyclerView
description: Building a RecyclerView with nested, horizontally scrolling sections in Kotlin.
excerpt: How to build a RecyclerView with nested, horizontally scrolling sections in Kotlin.
permalink: /blog/nested-horizontal-recyclerview/
---
<p align="center">
<img src="/images/Groovy.png" width="320"><br>
</p>
This UI design is quite commonly used in apps and I recently built this layout for Hike Discover. This tutorial details how to build a vertically scrolling `RecyclerView` with sections of horizontally scrolling `RecyclerView`s within it.

For the purpose of this tutorial we're going to implement a more basic (boring) version of the same layout which looks something like this.

<p align="center">
<img src="/images/NestedHorizontalRecyclerView.png" width="320"><br>
</p>

This app displays 5 sections with 10 items each, each item consisting of an image and some text. A section is defined by its model class `SectionModel`.

{% highlight kotlin %}
data class SectionModel(val type: Int,
                        val name: String,
                        val data: List<Int>)
{% endhighlight %}

A basic Activity called `MainActivity` hosts a `RecyclerView` called `recyclerview_main` which has an adapter called `MainAdapter`.

{% highlight kotlin %}
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        setupView()
    }

    private fun setupView() {
        recyclerview_main.apply {
            layoutManager = LinearLayoutManager(this@MainActivity)
            adapter = MainAdapter(this@MainActivity)
        }

        // Some dummy data. Don't ask.
        val sectionedData = listOf(
                SectionModel(HORIZONTAL_LIST, "FIFTIES", listOf(50..59).flatMap { it }),
                SectionModel(HORIZONTAL_LIST, "SIXTIES", listOf(60..69).flatMap { it }),
                SectionModel(HORIZONTAL_LIST, "SEVENTIES", listOf(70..79).flatMap { it }),
                SectionModel(HORIZONTAL_LIST, "EIGHTIES", listOf(80..89).flatMap { it }),
                SectionModel(HORIZONTAL_LIST, "NINETIES", listOf(90..99).flatMap { it })
        )

        (recyclerview_main.adapter as MainAdapter).sections = sectionedData
    }
}
{% endhighlight %}

`MainAdapter` is responsible for managing the vertically scrolling `RecyclerView`. It inflates a `horizontalView` for each section and binds the corresponding data to the `HorizontalViewHolder`.

{% highlight kotlin %}
class MainAdapter(val context: Context) : RecyclerView.Adapter<RecyclerView.ViewHolder>() {

    var sections = listOf<SectionModel>()
        set(value) {
            field = value
            notifyDataSetChanged()
        }

    private val recycledViewPool = RecyclerView.RecycledViewPool()

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): RecyclerView.ViewHolder {
        return when (viewType) {
            HORIZONTAL_LIST -> {
                val horizontalView = LayoutInflater.from(parent.context)
                        .inflate(R.layout.horizontal_section, parent, false)
                HorizontalViewHolder(horizontalView)
            }
            else -> {
                throw Exception("Unexpected viewType")
            }
        }
    }

    override fun onBindViewHolder(holder: RecyclerView.ViewHolder, position: Int) {
        when (getItemViewType(position)) {
            HORIZONTAL_LIST -> {
                holder.itemView.textview_section_name.text = sections[position].name
                holder.itemView.recyclerview_horizontal.apply {
                    adapter = HorizontalRecyclerAdapter(sections[position])
                    layoutManager = LinearLayoutManager(this@MainAdapter.context, LinearLayoutManager.HORIZONTAL, false)
                    setRecycledViewPool(this@MainAdapter.recycledViewPool)
                }
            }
            else -> { throw IllegalArgumentException("viewType in onBindViewHolder is faulty.") }
        }
    }

    override fun getItemCount(): Int {
        return sections.size
    }

    override fun getItemViewType(position: Int): Int {
        return sections[position].type
    }

    class HorizontalViewHolder(view: View) : RecyclerView.ViewHolder(view)
}
{% endhighlight %}

The UI for the horizontal section is defined as such:

{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical">

    <TextView
        android:id="@+id/textview_section_name"
        tools:text="Section name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="8dp"
        android:textSize="16sp"/>

    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerview_horizontal"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>

</LinearLayout>
{% endhighlight %}

A couple of things to note here.
  * `HORIZONTAL_LIST` is the `viewType` for a horizontally scrolling RecyclerView. If the overall layout requires other different kind of  `viewType`s, simply add a new section type to the `sections` list and add conditions just like `HORIZONTAL_LIST` condition to the `when` control flows to inflate/bind the corresponding view.
  * We're setting a custom `RecycledViewPool` for each section's `RecyclerView`. This is to prevent each view of the inner `RecyclerView`s to be inflated again when the user scrolls vertically. [Here's](https://proandroiddev.com/optimizing-nested-recyclerview-a9b7830a4ba7){:target="_blank"} a nice article for more detail.
  * The `LayoutManager` for each horizontally scrolling `RecyclerView` has the orientation set to `LinearLayoutManager.HORIZONTAL`.

Finally, an adapter is defined for each inner (horizontally scrolling) `RecyclerView` which is like any other `RecyclerView`. Easy peasy.

{% highlight kotlin %}
class HorizontalRecyclerAdapter(var sectionData: SectionModel) : RecyclerView.Adapter<RecyclerView.ViewHolder>() {

    class ViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView)

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): RecyclerView.ViewHolder {
        return ViewHolder(LayoutInflater.from(parent.context)
                .inflate(R.layout.recyclerview_horizontal_item, parent, false))
    }

    override fun getItemCount(): Int {
        return sectionData.data.size
    }

    override fun onBindViewHolder(holder: RecyclerView.ViewHolder, position: Int) {
        holder.itemView.item_textview.text = sectionData.data[position].toString()
    }
}
{% endhighlight %}

And that's that! A `RecyclerView` example that supports different viewTypes and nested, horizontally scrolling `RecyclerView`s inside it. Some basic UI code has been left out of the tutorial but the entire implementation can be found on my [Github](https://github.com/amanps/NestedHorizontalRecyclerView){:target="_blank"} if you'd like to work directly with it.

NOTE: This implementation has been recently migrated to Kotlin.
