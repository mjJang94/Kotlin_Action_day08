# 클래스 위임: by 키워드 사용

인터페이스를 구현할 때 by 키워드를 통해 그 인터페이스에 대한 구현을 다른 객체에 위임 중이라는 사실을 명시할 수 있다.

<pre><code>
class DelegatingCollection<T>( innerList: Collection<T> = ArrayList<T>()) : Collection<T> by innerList {}
</code></pre>

메소드 중 일부의 동작을 변경하고 싶은 경우 메소드를 오버라이드하면 컴파일러가 생성한 메소드 대신 오버라이드한 메소드가 쓰인다.   
기존 클래스가 가지고 있는 메소드에 위임하는 기본 구현으로 충분한 메소드의 경우에는 따로 오버라이드할 필요가 없다.   

이러한 기법을 사용해서 원소를 추가하려고 시도한 횟수를 기록하는 컬렉션을 구현해보자.

<pre><code>
class CountingSet<T>( val innerSet: MutableCollection<T> = HashSet<T>() ): MutableCollection<T> by innerSet { //---MutableCollection 구현을 innerSet에 위임
  var objectAdded = 0
  
  override fun add(element: T) : Boolean{
    objectsAdded++
    return innerSet.add(element)
   }
   
   override fun addAll(c: Collection<T>) : Boolean {
    objectsAdded += c.size
    return innerSet.addAll(c)
  }
  
  
>>> val cset = CountingSet<Int>()
>>> cset.addAll(listOf(1, 1, 2))
>>> println("${cset.objectsAdded} object were added, ${csat.size} remain")
3 object were added, 2 remain
</code></pre>

이때 CountingSet에 MutableCollection의 구현 방식에 대한 의존관계가 생기지 않았다.   
예를 들면 addAll을 처리할 때 루프를 돌면서 add를 호출할 수도 있지만, 최적화를 위해 다른 방식을 찾아볼 수 있다.

