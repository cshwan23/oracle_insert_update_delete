# oracle_insert_update_delete
oracle_insert_update_delete


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.130 직급벌 직급, 평군 나이 정렬조건은 직급 순서대로 나오기
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    select
    JIKUP
    ,round(avg(
        extract(year from sysdate)
        -
        extract( year from
            to_date(decode(substr(JUMIN_NUM,7,1),'1','19','2','19','20')||substr(JUMIN_NUM,1,6),'YYYYMMDD')
            )+1
        ),1
        ) "직원평균나이"
    from EMPLOYEE
    group by JIKUP
    order by  decode(JIKUP,'사장',1,'부장',2,'과장',3,'대리',4,5);
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.131 고객의 연령대별 인원수?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- 30대 2명
    --40대 4명
    --50대 4명
    select
        floor(
            ( extract(year from sysdate)
            -
            extract(
                year from
                to_date(decode(substr(JUMIN_NUM,7,1),'1','19','2','19','20')||substr(JUMIN_NUM,1,6),'YYYYMMDD')
                )+1
            )*0.1
        )||'0'||'대' "연령대별"
    ,count(*)||'명' "인원수"
    from CUSTOMER
    group by floor(
            ( extract(year from sysdate)
            -
            extract(
                year from
                to_date(decode(substr(JUMIN_NUM,7,1),'1','19','2','19','20')||substr(JUMIN_NUM,1,6),'YYYYMMDD')
                )+1
            )*0.1
        )||'0'||'대'

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- insert, update, delete
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    /*
    ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    insert 문
    ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    ▶︎ 테이블에 [*행*을 입력] 하는 명령이다
    ▶︎ 형식
        ----------------------------------------------------------
        insert into 테이블명[(칼럼명1, 칼럼명2, ~, 칼럼명 n)] values(입력값1,입력값2, ...입력값 n);
        ----------------------------------------------------------
        insert into 테이블명[(칼럼명1, 칼럼명2, ~, 칼럼명 n)] select 문
        ----------------------------------------------------------
        ▶︎ 데이터 입력 시 *NOT NULL, PK* 칼럼은 반드시 입력해야 하고 아닌 컬럼은 데이터 입력을 안 할 수도 있다.

    ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    update 문
    ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    ▶  테이블에 [컬럼값을 수정]하는 명령문이다. 즉 테이블의 *셀* 을 수정하는 명령문이다.
    ▶ ︎ 형식
        ----------------------------------------------------------
        update 테이블명 set 컬럼명1=수정값1, 컬럼명2=수정값2, ~, 컬럼명 n = 수정값 n where 조건절;
        ----------------------------------------------------------
        ▶ ︎ UNIQUE 또는 PK 컬럼의 데이터를 수정할 경우 중복되는 데이터로 수정되지 않는다.
        ▶ FK 컬럼의 데이터를 수정할 경우 PK에 없는 데이터로 수정되지 않는다.
        ▶ check 제약 조건이 걸린 컬럼의 데이터를 수정할 경우 check 계약 조건에서 지정된 데이터가 아닌 데이터로 수정되지 않는다.
        ▶ 수정값 자리에 서브쿼리가 올 수도 있다.
        ▶ <주의> where 조건절을 생략할 경우 모든 행이 수정된다.
        ▶ <주의> where 조건절은 이론적으로 생략이 가능하나 실무적으로 생략되는 경우는 거의 없다.

    ◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎
    delete 문
    ◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎◼︎
    ▶︎ 테이블의 [*행*을 삭제]하는 명령문이다. <주의> 셀 단위로 삭제하는 명령어가 아니다.
    ▶︎ 형식
        ----------------------------------------------------------
        delete from 테이블명 where 조건절; (*from 빼먹지 말자)
        ----------------------------------------------------------
        ▶ FK 컬럼이 참조하는 PK 컬럼의 데이터는 삭제되지 않는다.
        ▶ <참고> FK 컬럼이 on delete cascade 로 정의되어 있으면 PK 컬럼 데이터 삭제시 따라서 자동 삭제된다.
        ▶ <주의> where 조건절을 생략 할 경우 모든 행이 삭제된다.
        ▶ <주의> where 조건절은 이론적으로 생략이 가능하나 실무적으로 생략되는 경우는 거의 없다.
     */


    --++++++++++++++++++
    -- 1. insert
    --++++++++++++++++++
    insert into dept(dep_no,dep_name) values (50,'영업부'); -- loc가 not null 이라 에러
    insert into DEPT values (50,'영업부','서울'); -- dap_name 은 unique 라 중복 되면 에러

    select * from dept;
    rollback;
    --++++++++++++++++++
    -- 2. update
    --++++++++++++++++++
    -------------------------------
    -- 영업부의 위치를 서울로 바꾸고 싶다.
    -------------------------------
    -- update 문은 셀(세로)이 바뀌는데 where(행)가 없으면 (모든행의)셀 전체가 바뀌므로
    -- update 문은 항상 where 가 나와야한다. *
    update dept set loc='서울' where dep_no=20;

    -- 제약조건 not null , pk 에 부합되지 않으면 에러 발생한다.
    -- dep_name 에 unique가 걸려있기 때문에 이미 존재하는 자재부가 있어서 중복되면 안되어 error
    update dept set dep_name='자재부' where dep_no=20;

    -- 자료형이 넘어가면 안된다. varchar2(20)
    update dept set loc='제주도' where dep_no=20;

    -- update는 update가 될까 안될까의 문제다.
    -- employee.dep_no는 fk인데 dept.dep_no의 pk에 없는 데이터 50을 갖다 쓰고 있다.
    update EMPLOYEE set dep_no=50 where emp_no=20; -- 에러 o 업데이트 실패

    -- pk 에 30이 있으니 업데이트 된다.
    update EMPLOYEE set dep_no=30 where emp_no=20;

    -- emp_no가 22이 없지만. 오류는 아니다. 22번을 못찾을 뿐.
    update EMPLOYEE set dep_no=20 where emp_no=22; --에러 x 대상이 없어 업데이트 실패

    update EMPLOYEE set dep_no=50 where emp_no=22; --에러 x 대상이 없어 업데이트 실패

    select * from EMPLOYEE;
    select * from dept;
    rollback;
    --++++++++++++++++++
    -- 3. delete
    --++++++++++++++++++
    -- employee.emp_no=3 이 pk값을 다른테이블에서 참조하고 있지 않아서 지워진다.
    delete from EMPLOYEE where EMP_NO=1; -- 지워진다.

    -- employee.emp_no=3 이 pk값을 다른테이블에서 참조하고 있어서 안지워진다.
    delete from EMPLOYEE where EMP_NO=3;  -- 지워지지않는다.

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.132 employee 테이블에 '장보고', 40, '대리', 3500, '2012-05-28','8311091109310','01092499215',3
    -- 데이터를 입력하는 insert 구문은?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm

    ------------------
    -- 시퀀스로 만드는방법
    -- 문제점 : 오류나더라도 번호가 추가되어 다음에 인서트하면 번호가 붕뜨게 된다.
    ----------------
    insert into EMPLOYEE(
            emp_no, emp_name, dep_no, jikup, salary, hire_date, jumin_num, phone_num, mgr_emp_no
        )
        values(
            EMP_SQ.nextval, '장보고', 60, '대리', 3500, to_date('2012-05-28','YYYY-MM-DD'),'8311091109310','01092499215',3
        );
    -------------------------------------------------
    --직원의 최대 pk 번호 구하기(emp_no)
    -- max(emp_no)+1
    -- 처음들어가는경우에는 null 이므로
    --nvl(max(emp_no),0)+1
    -- 처음들어가는 경우엔 max값이 null일땐 0으로 바꿔버리고 1을더해준다.
    -------------------------------------------------
    insert into EMPLOYEE(
            emp_no, emp_name, dep_no, jikup, salary, hire_date, jumin_num, phone_num, mgr_emp_no
        )
        values(
            (select nvl(max(EMP_NO),0)+1 from EMPLOYEE), '장보고', 60, '대리', 3500, to_date('2012-05-28','YYYY-MM-DD'),'8311091109310','01092499215',3
        );

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.133 employee 테이블에서 주민번호 8203121977315, 이름 강감찬 직원의 직급을 주임으로 수정하려면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- 주민번호를 넣는 이유는 같은 이름이 두명이상일수있기때문.
    update EMPLOYEE set JIKUP='주임' where EMP_NAME ='강감찬' and JUMIN_NUM ='8203121977315';

    select * from EMPLOYEE;

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.134 employee 테이블에서 주민번호 8410031281312, 이름 공부해 직원의 연봉을 5% 인상하면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    update EMPLOYEE set SALARY=(SALARY*1.05) where EMP_NAME='공부해' and JUMIN_NUM='8410031281312';
    select * from EMPLOYEE;

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.135 employee 테이블에서 연봉 40000이상의 직원 연봉을 2% 삭감하면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    update EMPLOYEE set SALARY=(SALARY*0.98) where SALARY>4000;

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.136 평균 연봉 보다 적은 연봉자의 연봉을 50만원 인상하면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- 평균을 구하는 서브쿼리가 들어가야 한다.
    update EMPLOYEE set SALARY= (SALARY + 50) where SALARY < (select avg(SALARY) from EMPLOYEE);

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.137 담당 고객이 있는 직원의 급여를 5% 인상하면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    update EMPLOYEE set SALARY = SALARY * 1.05
    where EMP_NO in (select distinct EMP_NO from CUSTOMER where EMP_NO is not null );


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.138 연봉 서열 1위~5위까지의 5명의 연봉을 10% 인하하면?
    -- 정렬기준 : 연봉높은순서> 직급높은순서> 입사일빠른순서> 나이높은순서
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    update EMPLOYEE set SALARY = SALARY*0.9
    where EMP_NO in (
        select EMP_NO
        from (
                 select e.*, ROWNUM "RNUM"
                 from (
                          select emp_no
                          from EMPLOYEE
                          order by SALARY desc
                         ,decode(JIKUP,'사장',1,'부장',2,'과장',3,'대리',4,5) desc
                         ,HIRE_DATE asc
                         ,decode(substr(JUMIN_NUM,7,1),'1','19','2','19','20')||substr(JUMIN_NUM,1,6) asc
                      ) e
                 where ROWNUM <= 5
             )
        where RNUM >=1
    );
    select*from EMPLOYEE;
    rollback ;

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.139 인원수가 제일 많은 직급 중에 연봉 서열 1위 직원의 연봉을 1% 삭감하면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- 1. 제일 위의 한행만 뽑아내기 랭킹 1위 뽑아내기.(inlineview/rownum)
    -- 2.
    update EMPLOYEE set SALARY=SALARY*0.99
    where EMP_NO= (
                    select EMP_NO
                    from (
                             select emp_no
                                  , JIKUP
                                  , (select count(*) from EMPLOYEE e2 where e1.JIKUP = e2.JIKUP) "CNT"
                             from EMPLOYEE e1
                             order by CNT desc
                                    , decode(JIKUP, '사장', 1, '부장', 2, '과장', 3, '대리', 4, 5) asc
                                    , SALARY desc
                         )
                    where ROWNUM = 1
                    );
    select*from EMPLOYEE;

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.140 employee 테이블과 데이터를 그대로 복사하여 employee2 테이블을 만들면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- create table 테이블명B as select * from 테이블명A
    -- select * from 테이블 A 를검색한 결과를 그대로  테이블 B 로 새로 생성한다.
    -- 근데 과연 제약조건도 따라왔을까?
    -- pk, fk 는 안따라오고 not null 만 따라온다.
    create table employee2 as select * from employee;
    select * from employee2;


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.141 아래 조건을 만족하는 테이블 student 를 만들면?
    -- 칼럼명 => stu_no , stu_name, jumin_num
    -- 자료형 => number(3) , varchar2(20), char(13)
    -- create 말고 쉽게 빨리 만드는 방법
    -- alias 를 준다.
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    create table student as select emp_no "stu_no", emp_name "stu_name", jumin_num "JUMIN_NUM" from employee
    where 1=2;

    --- 가상테이블
    create view employee_vw1 as
        select * from employee
        order by
                 SALARY desc
                 ,decode(JIKUP,'사장',1,'부장',2,'과장',3,'대리',4,5) desc
                 ,HIRE_DATE asc
                 ,decode(substr(JUMIN_NUM,7,1),'1','19','2','19','20')||substr(JUMIN_NUM,1,6) asc;

    -- 직급별 평균나이 view(가상테이블) 만들기
    -- view 를 사용하는 목적 2가지
    -- 1번째 : 아주 긴 셀렉트 쿼리를 이름붙여 아주 쉽게 갖다쓰기 편하다.
    -- 2번째 : 보안 때문에
    create view xxx as
    select
    JIKUP
    ,round(avg(
        extract(year from sysdate)
        -
        extract( year from
            to_date(decode(substr(JUMIN_NUM,7,1),'1','19','2','19','20')||substr(JUMIN_NUM,1,6),'YYYYMMDD')
            )+1
        ),1
        ) "AVG_AGE"
    from EMPLOYEE
    group by JIKUP
    order by  decode(JIKUP,'사장',1,'부장',2,'과장',3,'대리',4,5);

    -------------------
    select  * from xxx;
    select  * from employee;
    create view ppp as
