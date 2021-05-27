# Hello-World
Just another Time- Honored Repository
SELECT Distinct  
--T0.[DocNum],  
 T0.[Series], T0.[ItemCode], SUm(T0.[PlannedQty]) as Plqty, Sum(T0.[CmpltQty]) as clqty, SUM(T0.[RjctQty]) as rjqty, T0.[CardCode],
--T0.[PostDate], T0.[DueDate], T0.[OriginNum]
---, t1.linenum,T1.[ItemCode],  T1.[PlannedQty], T1.[IssuedQty], T1.[UomCode]
T2.[CardCode], T2.[CardName], T2.[DocDate], T2.[DocNum], T2.[U_CustOrdDate]
,T2.[NumAtCard], T3.[ItemCode], T3.[Dscription], T3.[U_PODate] 
,T3.[U_PONumber], T3.[Price], T3.[DelivrdQty], T3.[GrssProfit], T0.[U_UPS], T3.[LineTotal]--, T4.[AvgPrice]
--,(Select Sum(a.TransValue) from OINM a where t1.itemcode = a.itemcode and a.[AppObjAbs] = t0.docentry) as "IssueValue"
--,(Select Sum(a.TransValue) from OINM a where t1.itemcode = a.itemcode and a.[AppObjAbs] = t0.docentry and a.transtype = 59) as "Receipt Value",
,(Select Sum(b.Linetotal) from OIGE a  inner join IGE1 b on a.DocEntry = b.DocEntry inner join owor c on b.baseref =  c.DocNum
inner join ordr d on d.DocNum = c.OriginNum
----inner join OIGE c on a.transid = c.transid inner join ige1
 where c.OriginNum = T2.DocNum) as "IssueValue"

--Case 
--When (Select Top 1 a.price from rdr1 a inner join ordr b on a.DocEntry = b.DocEntry where a.ItemCode = t3.ItemCode Order by a.LineNum desc) = 0
--Then (Select  Top 1 c.price from inv1 c inner join oinv d on c.DocEntry = d.DocEntry right join 
--dln1 e on e.DocEntry = c.BaseEntry right join rdr1 f on f.docentry = e.BaseEntry order by c.DocEntry desc)
--else (Select Top 1 a.price from rdr1 a inner join ordr b on a.DocEntry = b.DocEntry where a.ItemCode = t3.ItemCode Order by a.LineNum desc)
--end as InvPrice

,T4.ItmsGrpCod, T4.[ItemName], T4.OnHand

--,T5.LineTotal, T5.Quantity, T5.[PriceBefDi]
,(Select Sum(e.LineTotal)---- e.quantity, tb.DocNum,e.BaseRef,ta.DocNum, d.BaseRef,a.docnum, * 
from 
ORDR a  --ON a.DOCNUM = T0.ORIGINNUM 
inner JOIN RDR1 b ON a.[DocEntry] = b.[DocEntry]
inner join DLN1  d ON d.BaseEntry = b.DocEntry and d.BaseLine = b.LineNum---d.ItemCode = b.ItemCode
Left Join ODLN  Ta ON Ta.DocEntry = d.DocEntry
Left Join INV1  e ON e.BaseEntry = d.DocEntry and e.BaseLine = d.LineNum----e.ItemCode = d.ItemCode
Left Join OINV  Tb ON Tb.DocEntry = e.DocEntry
where a.DocEntry = T2.DocEntry
AND a.CANCELED = 'N'
AND Ta.CANCELED = 'N'
AND Tb.CANCELED = 'N' 
) as LineTotalinv


,(Select Sum(e.Quantity)   ---- e.quantity, tb.DocNum,e.BaseRef,ta.DocNum, d.BaseRef,a.docnum, * 
from 
ORDR a  --ON a.DOCNUM = T0.ORIGINNUM 
inner JOIN RDR1 b ON a.[DocEntry] = b.[DocEntry]
inner join DLN1  d ON d.BaseEntry = b.DocEntry and d.BaseLine = b.LineNum---d.ItemCode = b.ItemCode
Left Join ODLN  Ta ON Ta.DocEntry = d.DocEntry
Left Join INV1  e ON e.BaseEntry = d.DocEntry and e.BaseLine = d.LineNum----e.ItemCode = d.ItemCode
Left Join OINV  Tb ON Tb.DocEntry = e.DocEntry
where a.DocEntry = T2.DocEntry
AND a.CANCELED = 'N'
AND Ta.CANCELED = 'N'
AND Tb.CANCELED = 'N' 
) as QuantityInv



FROM 
OWOR T0  
--INNER JOIN WOR1 T1 ON T0.[DocEntry] = T1.[DocEntry] 
INNER JOIN 
ORDR T2  ON T2.DOCNUM = T0.ORIGINNUM 
INNER JOIN RDR1 T3 ON T2.[DocEntry] = T3.[DocEntry]
--left join DLN1  T5 ON T5.BaseEntry = T3.DocEntry and T5.ItemCode = T3.ItemCode
--Left Join ODLN  Ta ON Ta.DocEntry = T5.DocEntry
--Left Join INV1  T6 ON T6.BaseEntry = T5.DocEntry and T6.ItemCode = T5.ItemCode
--Left Join OINV  Tb ON Tb.DocEntry = T6.DocEntry
INNER JOIN OITM T4 ON T4.[ItemCode] = T3.[ItemCode]
where T0.[DocNum] < 20000
and T2.DocNum ='696' 
----AND T2.Docnum = '696'
--AND T2.CANCELED = 'N'
--AND Ta.CANCELED = 'N'
--AND Tb.CANCELED = 'N'
----AND T3.ItemCode = 'FG01055'
Group by

--T0.[DocNum],  
 T0.[Series], T0.[ItemCode], T0.[CardCode],
--T0.[PostDate], T0.[DueDate], T0.[OriginNum]
---, t1.linenum,T1.[ItemCode],  T1.[PlannedQty], T1.[IssuedQty], T1.[UomCode]
T2.[CardCode], T2.[CardName], T2.[DocDate], T2.[DocNum], T2.[U_CustOrdDate]
,T2.[NumAtCard], T3.[ItemCode], T3.[Dscription], T3.[U_PODate] 
,T3.[U_PONumber], T3.[Price], T3.[DelivrdQty], T3.[GrssProfit], T0.[U_UPS], T3.[LineTotal]--, T4.[AvgPrice]
--,(Select Sum(a.TransValue) from OINM a where t1.itemcode = a.itemcode and a.[AppObjAbs] = t0.docentry) as "IssueValue"
--,(Select Sum(a.TransValue) from OINM a where t1.itemcode = a.itemcode and a.[AppObjAbs] = t0.docentry and a.transtype = 59) as "Receipt Value",


--Case 
--When (Select Top 1 a.price from rdr1 a inner join ordr b on a.DocEntry = b.DocEntry where a.ItemCode = t3.ItemCode Order by a.LineNum desc) = 0
--Then (Select  Top 1 c.price from inv1 c inner join oinv d on c.DocEntry = d.DocEntry right join 
--dln1 e on e.DocEntry = c.BaseEntry right join rdr1 f on f.docentry = e.BaseEntry order by c.DocEntry desc)
--else (Select Top 1 a.price from rdr1 a inner join ordr b on a.DocEntry = b.DocEntry where a.ItemCode = t3.ItemCode Order by a.LineNum desc)
--end as InvPrice

,T4.ItmsGrpCod, T4.[ItemName], T4.OnHand, T2.Docentry


