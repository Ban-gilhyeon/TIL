
## LOMBOK

@Data 를 쓰는게 안좋다고 해서 @Getter @RequiredArgsConstructor를 썼는데 writeform에서 제목이 안넘어 왔었음 이유가 뭘까?
```java
//BoardController.class
@PostMapping("board/writepro")  
public String writepro(Board board) {  
    System.out.println(board.getTitle());  
  
    return " ";  
}
```

```html
<!--BoardWrite.html -->
<div class ="layout">  
    <form action ="/board/writepro" method="post">  
        <input name="title" type="text">  
        <textarea name="content"></textarea>  
        <button type="submit">작성</button>  
    </form>  
</div>
```