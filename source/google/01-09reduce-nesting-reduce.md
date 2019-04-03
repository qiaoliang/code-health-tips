## Reduce Nesting, Reduce Complexity

嵌套过深的代码对可读性有害，而且容易出错。 请在下面两个版本的代码中**试着找一找bug**：

<table class="my-bordered-table">
  <tbody>
<tr width=‘50%’>
      <td style="text-align: center;"><em>嵌套太多的代码</em></td>
      <td style="text-align: center;"><em>较少嵌套的代码</em></td>
</tr>
<tr><td bgcolor=LightPink>
<pre >response = server.Call(request)
if response.GetStatus() == RPC.OK:
  if response.GetAuthorizedUser():
    if response.GetEnc() == 'utf-8':
      if response.GetRows():
        vals = [ParseRow(r) for r in 
                response.GetRows()]
        avg = sum(vals) / len(vals)
        return avg, vals
      else:
        raise EmptyError()
    else:
      raise AuthError('unauthorized')
  else:
    raise ValueError('wrong encoding')
else:
  raise RpcError(response.GetStatus())</pre>
</td>
    <td style="background-color: #d9ead3;"><pre style="background-color: #d9ead3; border: 0px; color: black;">
response = server.Call(request)
if response.GetStatus() != RPC.OK:
  raise RpcError(response.GetStatus())
if not response.GetAuthorizedUser():
  raise ValueError('wrong encoding')
if response.GetEnc() != 'utf-8':
  raise AuthError('unauthorized')
if not response.GetRows():
  raise EmptyError()
vals = [ParseRow(r) for r in 
        response.GetRows()]
avg = sum(vals) / len(vals)
return avg, vals</pre>
</td>
  </tr>
</tbody></table>

答案："wrong encoding"和"unauthorized" 这两个Error Message写反了。**在重构后的版本中，这个bug更容易被发现，因为代码中，直接进行检查后就可以处理Error了。**

上面展示的重构技术被称为 *卫语句*。通过卫语句来检查某一条件，如果不满足就直接快速失败。它将计算逻辑与错误逻辑分离了。通过消除错误检查和处理之间的认知差距，它释放了心理加工能力。所以，**这个重构后的版本更容易读懂，也更容易维护。**

**下面是一些减少嵌套代码的原则**:

- 尽量让条件语句块短小。将内容限制在局部，有利于增加可读性。
- 当循环或分支超过两层深度时，就要考虑重构它了。
- 可以将嵌套逻辑抽取到另一个函数中。假如你需要从一个列表中取出每一个元素，而每个元素又都是一个列表（例如，一个协议缓冲区中还有重复的字段），你就可以定义一个函数来处理每个对象，而不是使用双嵌套循环。

减少嵌套会使代码更易读，从而更容易发现可能存在的错误，更快的开发迭代，以及更高的稳定性。只要允许，就进行简化！