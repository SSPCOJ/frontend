<template>
  <panel>
    <div slot="title">{{$t('m.Frequently_Asked_Questions')}}</div>
    <div class="content markdown-body">
      <ul>
        <li>입력과 출력은 어디서 하나요?
          <p>유저가 작성한 프로그램은 <b>표준 입력(<code>stdin</code>)</b> 으로부터 입력을 읽고, <b>표준 출력(<code>stdout</code>)</b> 으로 출력을 해야 해요.
            예를 들어, C++에서는 <code>cin</code>을 사용하여 입력받고, <code>cout</code>을 사용하여 출력할 수 있어요.
            Python에서는 <code>input</code>을 사용하여 입력받고, <code>print</code>를 사용해 출력할 수 있어요.

            사용자 프로그램이 <b>파일을 읽거나 쓰는 행위는 허용되지 않으며</b>,
            그렇게 할 경우 <b>런타임 에러(Runtime Error)</b>가 발생해요.
          </p>
        </li>
        <li>실행 시간은 무엇을 뜻하나요?
          <p>
            온라인 저지는 여러 개의 입력 파일로 <b>유저의 코드를 여러 번 테스트</b>할 수 있어요.
            각 입력 파일에 대해 <b>시간 제한 내에 올바른 정답을 출력한 경우</b>,
            표시되는 실행 시간은 <b>각 테스트 케이스에서 소요된 시간 중 가장 긴 시간</b>이에요.

            하지만 <b>어떤 입력에 대해서라도 정답을 출력하지 못하거나 시간 초과가 발생하면,
            표시되는 실행 시간은 의미가 없어요.</b>
          </p>
        </li>
        <li>Java 명세
          <p>
          모든 프로그램은 반드시 <code>Main</code> 클래스의 <code>static main</code> 메서드에서 시작해야 해요.
          <code>public</code> <b>클래스를 사용하지 마세요.</b> <code>Main</code> 클래스도 <code>public</code>으로 선언하면 <b>컴파일 에러</b>가 발생해요.
          </p>
        </li>
        <li>출력 형식
          <p>
            채점기는 <b>출력의 마지막 줄에서 공백 문자와 줄바꿈을 자동으로 제거(trim)</b>해요.
            그럼에도 불구하고 정답과 출력이 다를 경우, 결과는 <b>Wrong Answer(오답)</b>로 처리돼요.
          </p>
        </li>
      </ul>
    </div>
  </panel>
</template>

<script>
</script>

<style lang="less" scoped>
  .content {
    font-size: 16px;
    margin: 0 50px 40px 50px;
    > ul {
      list-style: disc;
      li {
        font-size: 16px;
        margin-top: 20px;
        &:first-child {
          margin-top: 0;
        }
        p {
          font-size: 14px;
          margin-top: 5px;
        }
      }
    }
  }
</style>
