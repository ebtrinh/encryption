<script lang="ts">
	import { stringify } from "postcss";
    let cancompute = true;
	let encryption = '';
    let decryption = '';
    let questions: number = 0;
    let miradd: string[] = [];
    let mirroredText: string[][] = [];
    let conversion: number[][] = [];
    let convadd: any[] = [];
    let finalEncryption: string = '';
    let teuext: string = '';
    let letToNumDict: Record<string,number> = {
        'a': 100,'b': 200,'c': 300,'d': 400,'e': 500,'f': 600,'g': 700,'h': 800,'i': 900,'j': 1000,'k': 1100,'l': 1200,'m': 1300,'n': 1400,'o': 1500,'p': 1600,'q': 1700,'r': 1800,'s': 1900,'t': 2000,'u': 2100,'v': 2200,'w': 2300,'x': 2400,'y': 2500,'z': 2600
    }
    let mirroredNums: Record<string,string> = {
        '1': '1', '2':'Z', '3':'B', '4':'h', '5':'S', '6':'9', '7':'L', '8':'8', '9':'6', '0':'0', '?':'¿'
        
    }
    let splitNums: Record<string,string[]> = {
        '1': ['‘', '|'], '2':['‘', '/'], '3':['E','B'], '4':['L', '/'], '5':['F','o'], '6':['/','o'], '7':['`','/'], '8':['d','P'], '9':['"','/'], '0':['(',')'], '-':['-','-'], '|':['|','|'],'?':['`',';'] 
    }
    let unsplitNums: Record<string,string> = {
        '‘|': '1', '‘/':'2', 'EB':'3', 'L/':'4', 'Fo':'5', '/o':'6', '`/':'7', 'dP':'8', '"/':'9', '()':'0', '--':'-', '||':'|', '`;':'?'
    }
    let reptnums: Record<string,number> = {
    }
    function encrypt(text: string) {
        convadd = [];
        conversion = [];
        reptnums = {};
        let addedString: string = '';
        cancompute = true;
        for (let i = 0; i < text.length; i++) {
            if (text[i] != ' ') {
                if (letToNumDict[text[i]] != undefined) {
                    if (reptnums[text[i]] != undefined) {
                        addedString = (letToNumDict[text[i]]-reptnums[text[i]]).toString();
                        reptnums[text[i]]+=1
                        if (reptnums[text[i]]==100){
                            questions++
                            reptnums[text[i]]=0;
                        }
                        for (let j = 0; j < questions; j++) {
                            addedString+='?';
                        }
                        convadd.push(addedString)   
                        ;
                    }
                    else {
                        
                        convadd.push(letToNumDict[text[i]].toString());
                        reptnums[text[i]] = 1;
                        
                    }
                }
                else {
                    cancompute = false;
                    break;
                }
            }
            else {
                
                conversion.push(convadd);
                convadd = [];    

            }
            
        }
        conversion.push(convadd);
        convadd = []; 
        if (cancompute) {
            console.log(conversion);
        }
        else {
            decryption = 'Invalid input';
        }
        


        mirroredText = [];
        miradd = [];
        let moir: string = '';
        let skib: string = '';
        for (let i = 0; i < conversion.length; i++) {
                for (let j = 0; j < conversion[i].length; j++) {
                    for (let k = 0; k < conversion[i][j].toString().length; k++) {
                        moir = conversion[i][j].toString();
                        skib=mirroredNums[moir[k]]+skib;
                        console.log(skib, 'skib')
                    }
                    
                    miradd.unshift(skib);
                    skib=''
                    console.log(miradd, 'miradd')
                }
                console.log(mirroredText, 'mirroredText')
                mirroredText.unshift(miradd);
                miradd=[]
                moir=''
        }

        mirroredText.join("|");
        conversion.join("|");
        let finMirText: string = ''
        let finConv: string = ''
        for (let i = 0; i < mirroredText.length; i++) {
            finMirText+=mirroredText[i].join('-');    
            if (i+1 != mirroredText.length) {
                finMirText+='|';    
            }
        }
        for (let i = 0; i < conversion.length; i++) {
            finConv+=conversion[i].join('-');    
            if (i+1 != conversion.length) {
                finConv+='|';    
            }
        }

        console.log(finMirText, 'mirroredText')
        console.log(finConv, 'conversion')
        let addFinal: string = '';
        for (let i = 0; i < finConv.length; i++) {
            addFinal = '['
            addFinal+=splitNums[finConv.charAt(i)][0];
            addFinal+=finMirText.charAt(i);
            addFinal+=splitNums[finConv.charAt(i)][1];
            addFinal += ']' 
            console.log(addFinal) 
            encryption+=addFinal;
        }
        console.log(encryption, 'finalEncryption')
        



        
    }

    let convText: string = '';
    function decrypt(text: string) {
        for (let i = 0; i < text.length/5; i++) {
            if (text.charAt(i*5) == '[' && text.charAt(i*5+4) == ']') {
                convText+=unsplitNums[text.charAt(i*5+1)+text.charAt(i*5+3)];
            }
            else {
                cancompute = false;
                break;
            }
        }
        
        if (cancompute) {
            decryption = convText;
            console.log(convText)
        }
        else {
            decryption = 'Invalid input';
        }
        let finalText = ''
        for (let i = 0; i < convText.length; i++) {
                if (convText.charAt(i) != '?') {
                    finalText+=convText.charAt(i-1);
                }
            }
        }
    }
    let tEuext: string = '';
</script>

<h1>EKEncryption</h1>
<p>no punctuation</p>
<p>no capitalization</p>
<div>
    <textarea rows="4" cols="50" bind:value={teuext} placeholder="enter text"></textarea>
</div>

<button on:click={() => encrypt(teuext)}>Encrypt</button>
<p>{encryption}</p>
<div>
    <textarea rows="4" cols="50" bind:value={tEuext} placeholder="enter encrypted data"></textarea>
</div>

<button on:click={() => decrypt(tEuext)}>Decrypt</button>
<p>{decryption}</p>


