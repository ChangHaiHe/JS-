##CSS3
	不定宽高垂直居中
	方案一：
		.wraper{
			position:absolute;
			top:50%;
			left:50%;
			-webkit-transform:translate(-50%,-50%);
		}
	方案二：
		.wraper{
			display:-webkit-flex;
			justify-content:center; //水平居中
			align-items:center; //垂直居中
		}
	方案三：
		.detailTitle{
			width:100%;
			font-weight: bold;
			min-height:@defaultHeaderHeight;
			background: #F5F5F5;
			display:table;
			span{
				width:100%;
				display:table-cell;
				vertical-align:middle;
				line-height:1;
				font-size:@defaultTextSize - 5px;
				text-align: center;
			}
		}
	
	超过字数...
		.fn-text-overflow {
				overflow: hidden;
				text-overflow: ellipsis;
				white-space: nowrap;
		}

	calc();

	阴影
	第几个儿子
	兄弟选择器
	三角
