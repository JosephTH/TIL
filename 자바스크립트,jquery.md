# DOM 조작하기

1. **$()**
   1. **class => .**, $('.')
   2. **id => #**, $('#search').remove()
   3. **attributes** => []
   4. **html_tag** => 그냥 text(‘p’,‘a’,‘span’)
2. **eq, :eq,** 
   1. $('span.an_icon').eq(6).css('display', 'none')
3. **text, html, val**
   1. $('.apln_item').eq(2).html() => a태그 출력
   2. $('.apln_item').eq(2).text() => a태그 내 글자 출력
   3. $('.apln_link').eq(0).text('<p>좋은아침입니다.</p>') => 내용을 text화 시켜서 웹에 출력
   4. $('.apln_link').eq(1).html('<p>좋은아침입니다.</p>') =>내용을 html 형식으로 웹에 출력
   5. $("#query").val('좋은 아침입니다') => input 한정!
4. **find**
   1. $('ul.an_l > li > a > span.an_icon')
   2. $('ul').find('span.an_icon')
5. **first, last, prev, next, parent, children**
   1. $('ul.an_l > li > a > span.an_icon').parent().text("메일")
6. **append, prepand, after, before(전체의 맨앞, 맨뒤, 객체의 앞, 뒤)**
   1. $("#PM_ID_serviceNavi").append('<li class="an_item">cc</li>')
   2. $("#PM_ID_serviceNavi").prepend('<li class="an_item">bb</li>')
7. **remove()**, hide(), show(), toggle()
   1. $("#search").remove()
8. **toggle()**
9. **replaceWIth()**
   1. $('ul.an_l > li > a').replaceWith("aa") => element 자체를 바꿔버림
10. **attr(k), attr(k,v);**
   1. $("a[data-clk='top.logo']").attr('href')
   2. $("a[data-clk='top.logo']").attr('data-clk')
   3. $("a[data-clk='top.logo']").attr('data-clk', 'bottom.logo')
   4. $("a[data-clk='bottom.logo']").attr('href', 'http://www.daum.net')
11. **on**
    1. $('a').on('mouseover', function() {alert('why!')})
    2. $('a').mouseover(function() {alert('why!')}) 이건 별로? => 새로운 엘러먼트가 추가되었을 때 작동 안함, 위에도 마찬가지
    3. $(document).on('mouseover', 'a', function() {alert('why!')})
       1. **Delegated event handlers** have the advantage that they can process events from *descendant elements* that are added to the document at a later time. By picking an element that is guaranteed to be present at the time the delegated event handler is attached, you can use delegated events to avoid the need to frequently attach and remove event handlers. This element could be the container element of a view in a Model-View-Controller design, for example, or `document` if the event handler wants to monitor all bubbling events in the document. The `document` element is available in the `head` of the document before loading any other HTML, so it is safe to attach events there without waiting for the document to be ready.