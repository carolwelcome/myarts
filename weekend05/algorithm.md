class Solution {
    public int findContentChildren(int[] g, int[] s) {
        //首先对数数组进行排序。
        int count=0;
        for(int i=0;i<g.length;i++){
            for(int j=0;j<s.length;j++){
                if(g[i]<=s[j]&&s[j]>0){                    
                    count++;
                   //System.out.println("第"+(i+1)+"个孩子,得到满足，他的胃口是:"+g[i]+",他分到第"+(j+1)+"号饼干，该饼干大小是:"+s[j]);
                    s[j]=0;
                    break;
                }
            }
        }
        return count;
    }
}


如何实现快速排序