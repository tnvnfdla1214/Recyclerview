# Recyclerview

 안드로이드 앱을 개발 하다보면 옛날에는 ListView(리스트뷰)를 많이 썼지만 요즘엔 리스트뷰의 거의 모든 기능을 RecyclerView(리사이클러뷰)로 할 수 있기때문에 대부분 RecyclerView를 많이 사용합니다.

 RecyclerView는 ListView보다 향상된 성능을 제공하는데 RecyclerView란 이름에서도 알 수 있듯이 Adapter의 ViewHolder를 이용하여 RecyclerView 내의 View를 재활용하여 사용합니다. 커스터마이징 하기에도 훨씬 좋아졌고 LayoutManager를 이용하여 ListView와 GridView를 표현할 수 있습니다.
 
 RecyclerView를 만들기 위해서는 RecyclerView, RecycclerView의 ItemView, Adapter, Data Class 4가지가 필요합니다.
 
 사실 당연히 알아야하는 파트이기 때문에 간단히만 하고 넘어가겠습니다.
 ***
 ### :wrench: 프로젝트 사용 사례
 
<img src = "https://user-images.githubusercontent.com/48902047/132084102-25a938ae-9a14-48b5-9b9d-bfea3f02c16c.jpg" width="20%" height="20%"> <img src="https://user-images.githubusercontent.com/48902047/122349509-4a5bd900-cf87-11eb-9c38-48a2e6dc713b.jpg" width="20%"> </img> <img src="https://user-images.githubusercontent.com/48902047/122348988-b8ec6700-cf86-11eb-9ae0-a834e148f52b.jpg" width="20%"></img>
 
+ [조치원 수호대](https://github.com/tnvnfdla1214/homemade_guardian) 
  + 커스텀 앨범 사진 리스트
  + 게시물 리스트
  + 댓글 리스트
  + 채팅 기능
+ [MVVM사용해보기](https://github.com/tnvnfdla1214/MvvmExample)
  + 등록된 사람 리스트
***
### :lollipop: 설명 (MVVM 사용해보기)

#### 라이브러리 추가

먼저 build.gradle 에 라이브러리를 추가합니다.

```Kotlin
/*build.gradle*/

dependencies {
    .
    .
    
    // recyclerview
    implementation "androidx.recyclerview:recyclerview:1.0.0"
    .
    .
}

```
#### xml 추가

xml에 Recyclerview를 알맞게 추가합니다.

```Kotlin
/*activiy_main.xml*/

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".view.MainActivity"
    android:orientation="vertical"
    android:gravity="center_horizontal">


    <androidx.recyclerview.widget.RecyclerView
        android:layout_width="match_parent"
        android:layout_height="630dp"
        android:layout_marginTop="16dp"
        android:id="@+id/main_recycleview"
        tools:listitem="@layout/item_contact"
        app:layout_constraintBottom_toTopOf="@+id/main_button"/>
        
        .
        .
        .

</LinearLayout>
```
#### item 추가

리사이클러뷰에 들어갈 item을 추가합니다.

```Kotlin
/*item_contact.xml*/

<?xml version="1.0" encoding="utf-8"?>
<androidx.cardview.widget.CardView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_margin="4dp"
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal">

        <TextView
            android:layout_width="50dp"
            android:layout_height="wrap_content"
            android:id="@+id/item_tv_initial"
            android:textSize="30dp"
            android:padding="4dp"
            android:background="@android:color/darker_gray"
            tools:text="H"
            android:gravity="center"/>

        <LinearLayout
            android:layout_width="300dp"
            android:layout_height="match_parent"
            android:orientation="vertical"
            android:layout_marginLeft="10dp"
            android:gravity="center_horizontal">
            <TextView
                android:layout_width="match_parent"
                android:layout_height="30dp"
                android:id="@+id/item_tv_name"
                android:textSize="20dp"
                android:textStyle="bold"
                android:text="ASDFGHJKL"
                android:gravity="center_horizontal"/>

            <TextView
                android:id="@+id/item_tv_number"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                tools:text="999-888-777"
                android:gravity="center_horizontal"/>
        </LinearLayout>

    </LinearLayout>

</androidx.cardview.widget.CardView>
```
#### Adqpter Class 만들기

RecyclerView의 Adapter답게 RecyclerView.Adapter를 상속받아 사용합니다.

해당 item의 요소를 ViewHolder 를 통해 bind합니다.

```Kotlin
/*ContactAdapter.java*/

class ContactAdapter(val contactItemClick: (Contact) -> Unit, val contactItemLongClick: (Contact) -> Unit)
    : RecyclerView.Adapter<ContactAdapter.ViewHolder>() {
    private var contacts: List<Contact> = listOf()

    override fun onCreateViewHolder(parent: ViewGroup, i: Int): ViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item_contact, parent, false)
        return ViewHolder(view)
    }

    override fun getItemCount(): Int {
        return contacts.size
    }

    override fun onBindViewHolder(viewHolder: ViewHolder, position: Int) {
        viewHolder.bind(contacts[position])
    }

    inner class ViewHolder(itemView: View): RecyclerView.ViewHolder(itemView) {
        private val nameTv = itemView.findViewById<TextView>(R.id.item_tv_name)
        private val numberTv = itemView.findViewById<TextView>(R.id.item_tv_number)
        private val initialTv = itemView.findViewById<TextView>(R.id.item_tv_initial)

        fun bind(contact: Contact) {
            nameTv.text = contact.name
            numberTv.text = contact.number
            initialTv.text = contact.initial.toString()

            itemView.setOnClickListener {
                contactItemClick(contact)
            }

            itemView.setOnLongClickListener {
                contactItemLongClick(contact)
                true
            }
        }
    }

    fun setContacts(contacts: List<Contact>) {
        this.contacts = contacts
        notifyDataSetChanged()
    }

}
```

#### Contact 만들기

Contact를 만들어 item에 들어갈 요소를 정의 합니다.

```Kotlin
/*Contact.java*/

@Entity(tableName = "contact")
data class Contact(
    @PrimaryKey(autoGenerate = true)
    var id: Long?,

    @ColumnInfo(name = "name")
    var name: String,

    @ColumnInfo(name = "number")
    var number: String,

    @ColumnInfo(name = "initial")
    var initial: Char
) {
    constructor() : this(null, "", "", '\u0000')
}
```


#### MainActivity 만들기

해당 어뎁터를 리사이클러뷰에 연결 시키고 가타 설정을 해줍니다.

```Kotlin
/*MainActivity.java*/

class MainActivity : AppCompatActivity() {

    private lateinit var contactViewModel: ContactViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Set contactItemClick & contactItemLongClick lambda
        val adapter = ContactAdapter({ contact ->
            val intent = Intent(this, AddActivity::class.java)
            intent.putExtra(AddActivity.EXTRA_CONTACT_NAME, contact.name)
            intent.putExtra(AddActivity.EXTRA_CONTACT_NUMBER, contact.number)
            intent.putExtra(AddActivity.EXTRA_CONTACT_ID, contact.id)
            startActivity(intent)
        }, { contact ->
            deleteDialog(contact)
        })

        val lm = LinearLayoutManager(this)
        main_recycleview.adapter = adapter
        main_recycleview.layoutManager = lm
        main_recycleview.setHasFixedSize(true)
        
        .
        .
        .
    }
}
```

