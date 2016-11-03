---
title: React Demo
date: 2016-10-28 09:42:30
tags: ReactJS
---
[TOC]
## React.findDOMNode()
React.findDOMNode()从组件获取真实DOM的节点。

    var MyComponent = React.createClass({
            handleClick:function(){
                React.findDOMNode(this.refs.myTextInput).focus();
            },
            render:function(){
                return (
                    <div>
                        <input type="text" ref="myTextInput" />
                        <input type="button" value="Focus the text input" onClick={this.handleClick}/>
                    </div>
                );
            }
        });
        React.render(
            <MyComponent />,
            document.getElementById('example')
        );

## ref
ref与React.findDOMNode()结合使用，该功能是为了结合现有非React类库，通过ref/refs可以获取得到组件实例，进而取得原生节点。

    var Test = React.createClass({
        componentDidMount(){
          alert(React.findDOMNode(this.refs.content).innerHTML);
        },
        render(){
            return <div>
                <h3>header</h3>
                <div ref='content'>content</div>
            </div>;
        }
    });

    React.render(<Test />,document.getElementById('example'));

##  this.state

    var LikeButton = React.createClass({
            getInitialState:function(){
                return {liked:false};
            },
            handleClick:function(event){
                this.setState({liked: !this.state.liked});
            },
            render:function(){
                var text = this.state.liked ? 'like' : 'haven\'t liked';
                return (
                    <p onClick={this.handleClick}>
                        You {text} this.Click to toggle.
                    </p>
                ); 
            }
        });
        React.render(
            <LikeButton />,
            document.getElementById('example')
        );

## 获取表单值
用户在表单填入的内容，属于用户跟组件的互动，所以不能用this.props读取。

      var Input = React.createClass({
        getInitialState:function(){
            return {value:'Hello!'};
        },
        handleChange:function(event){
            this.setState({value:event.target.value});
        },
        render:function(){
            var value = this.state.value;
            return (
                <div>
                    <input type="text" value={value} onChange={this.handleChange} />
                    <p>{value}</p>
                </div>
            );
        }
    });
    React.render(<Input />,document.getElementById('example'));
上面代码中，文本输入框的值，不能用this.props.value读取，而要定义一个onChange事件的回调函数，通过event.target.value读取用户输入的值。textarea元素、select元素、radio元素都属于这种情况。

 - value/checked属性设置后，用户输入无效
 - textarea的值要设置在value属性上
 - select的value属性可以是数组，不建议使用option的selected属性
 - input/textarea的onChange用户每次输入都会触发
 - radio/checkbox点击后也会触发onChange
 
如果设置了value属性，组件变为受控组件，用户无法输入，除非程序改变value属性。受控组件可以通过监听handleChange事件，结合state来改变input值。

## ajax
具体见**评论框示例中**，ajax也可直接写在componentDidMount中。

## 评论框Demo

    // 代码摘自官网教程
    var CommentBox = React.createClass({
        getInitialState: function() {
            return {data: []};
        },
      loadCommentsFromServer: function() {
          getInitialState: function() {
            return {data: []};
          },
        $.ajax({
          url: this.props.url,
          dataType: 'json',
          cache: false,
          success: function(data) {
            this.setState({data: data});
          }.bind(this),
          error: function(xhr, status, err) {
            console.error(this.props.url, status, err.toString());
          }.bind(this)
        });
      },
      
      componentDidMount: function() {
        this.loadCommentsFromServer();
        setInterval(this.loadCommentsFromServer, this.props.pollInterval);
      },
      render: function() {
        return (
          <div className="commentBox">
            <h1>Comments</h1>
            <CommentList data={this.state.data} />
            <CommentForm />
          </div>
        );
      }
    });

    var CommentList = React.createClass({
      render: function() {
        var commentNodes = this.props.data.map(function(comment) {
          return (
            <Comment author={comment.author} key={comment.id}>
              {comment.text}
            </Comment>
          );
        });
        return (
          <div className="commentList">
            {commentNodes}
          </div>
        );
      }
    });

    var CommentForm = React.createClass({
      render: function() {
        return (
          <form className="commentForm">
            <input type="text" placeholder="Your name" />
            <input type="text" placeholder="Say something..." />
            <input type="submit" value="Post" />
          </form>
        );
      }
    });

    ReactDOM.render(
      <CommentBox url="/api/comments" pollInterval={2000} />,
      document.getElementById('content')
    );
若child需要向parent传递值，则

    //修改以下两个模块的代码
    var CommentForm = React.createClass({
      getInitialState: function() {
        return {author: '', text: ''};
      },
      handleAuthorChange: function(e) {
        this.setState({author: e.target.value});
      },
      handleTextChange: function(e) {
        this.setState({text: e.target.value});
      },
      handleSubmit: function(e) {
        e.preventDefault();
        var author = this.state.author.trim();
        var text = this.state.text.trim();
        if (!text || !author) {
          return;
        }
        this.props.onCommentSubmit({author: author, text: text});
        this.setState({author: '', text: ''});
      },
      render: function() {
        return (
          <form className="commentForm" onSubmit={this.handleSubmit}>
            <input
              type="text"
              placeholder="Your name"
              value={this.state.author}
              onChange={this.handleAuthorChange}
            />
            <input
              type="text"
              placeholder="Say something..."
              value={this.state.text}
              onChange={this.handleTextChange}
            />
            <input type="submit" value="Post" />
          </form>
        );
      }
    });
    var CommentBox = React.createClass({
      loadCommentsFromServer: function() {
        $.ajax({
          url: this.props.url,
          dataType: 'json',
          cache: false,
          success: function(data) {
            this.setState({data: data});
          }.bind(this),
          error: function(xhr, status, err) {
            console.error(this.props.url, status, err.toString());
          }.bind(this)
        });
      },
      handleCommentSubmit: function(comment) {
        var comments = this.state.data;
        <!-- Optimistically set an id on the new comment. It will be replaced by an id generated by the server. In a production application you would likely not use Date.now() for this and would have a more robust system in place.-->
        comment.id = Date.now();
        var newComments = comments.concat([comment]);
        this.setState({data: newComments});
        $.ajax({
          url: this.props.url,
          dataType: 'json',
          type: 'POST',
          data: comment,
          success: function(data) {
            this.setState({data: data});
          }.bind(this),
          error: function(xhr, status, err) {
            this.setState({data: comments});
            console.error(this.props.url, status, err.toString());
          }.bind(this)
        });
      },
      getInitialState: function() {
        return {data: []};
      },
      componentDidMount: function() {
        this.loadCommentsFromServer();
        setInterval(this.loadCommentsFromServer, this.props.pollInterval);
      },
      render: function() {
        return (
          <div className="commentBox">
            <h1>Comments</h1>
            <CommentList data={this.state.data} />
            <CommentForm onCommentSubmit={this.handleCommentSubmit} />
          </div>
        );
      }
    });
    