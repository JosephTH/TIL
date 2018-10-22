```ruby
resources :comments, only: [:create, :destroy, :edit, :update] 

#view file
<%= link_to(fa_icon('edit'), edit_comment_path(comment), remote: true, data:{id: comment.id}) if comment.user == current_user %>

      <%= link_to(fa_icon('trash'), comment_path(comment), method: :delete) if comment.user == current_user %>

#_form.html.erb
<%= form_for(comment, html: {style: "display: inline"}, remote: true) do |f| %>
<%= f.text_field :content %>
<%= f.submit %>
<button class="cancel">cancel</button>
<% end %>
 
def edit
@comment = Comment.find(params[:id])
respond_to do |format|
format.js
format.html
end
end

#edit.js.erb
var li = $('li[data-id=<%=params[:id]%>]');
var form = "<%=j(render 'form', comment: @comment)%>";
var content = li.children().eq(1);
content.hide();
content.before(form);




#update.js.erb
var li = $('li[data-id=<%=params[:id]%>]');
var edit_form = $("form[id='edit_comment_<%=params[:id]%>'");
var content = li.children().eq(1);
content.text(<%=@comment.content%>);
content.show();


#javascript, cancel
<script>
  $(document).on('click', '.cancel', function(e){
    e.preventDefault();
    $(this).parent().next().show();
    $(this).parent().remove();
  });
</script> 

  def update
    @comment= Comment.find(params[:id])
    @comment.update(content: comment_params[:content])
    respond_to do |format|
      format.js
      format.html {redirect_back(fallback_location: root_path)}
    end
  end 

//= require best_in_place

gem 'best_in_place'
```

