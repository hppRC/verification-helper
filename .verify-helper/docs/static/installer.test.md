# 競プロライブラリに自動で verify をしてくれる CI を簡単に設定できる Web ページ

## verify を自動でしてくれるように設定するには

手順:

1.  <form>
        <label>競プロライブラリの GitHub のレポジトリの URL を右の textbox に入力: </label><input type="text" id="input"
            placeholder="https://github.com/beet-aizu/library" value="https://github.com/beet-aizu/library" size="48">
    </form>

1.  ページ <a id="output" target="_blank"></a> を開き、下の方にある緑の `Commit new file` ボタンを押す
1.  <a href="https://github.com/kmyk/online-judge-verify-helper/blob/master/example.test.cpp">example.test.cpp</a> のように `#define PROBLEM "https://..."` が書かれている `hoge.test.cpp` のようなファイル名の C++ コードを追加する (<a id="output2" target="_blank">例を自動で追加するリンク</a>)
1.  <a id="output3" target="_blank">GitHub Actions <img id="output7"></a> のページから結果を確認する
1.  (おまけ) <code>README.md</code> に <code id="output4"></code> と書き足す (バッチ <a id="output5" target="_blank"><img id="output6"></a> が貼られる)

## ドキュメントが自動生成されるように設定するには

手順:

1.  verify を自動でしてくれるように設定する
1.  [コマンドライン用の個人アクセストークンを作成する - GitHub ヘルプ](https://help.github.com/ja/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line) に従い、権限 `repo` を持った Personal Access Token を生成する
1.  [Creating and using encrypted secrets - GitHub ヘルプ](https://help.github.com/ja/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets#creating-encrypted-secrets) に従い、作成した Personal Access Token を `GH_PAT` という名前の secret として保存する

(注意: この設定がなくてもドキュメントのデータ自体は自動で生成され `gh-pages` branch へ push されます。しかし GitHub Actions の制約のために GitHub Pages の更新までは行われません。その場合は `gh-pages` branch へ手動で空の commit などを push すれば GitHub Pages の更新が行われます。この制約はおそらくは GitHub Actions の設定ミスによる無限ループを抑制するためのものです。)

<script>
    const input = document.getElementById("input");
    const output = document.getElementById("output");
    const output2 = document.getElementById("output2");
    const output3 = document.getElementById("output3");
    const output4 = document.getElementById("output4");
    const output5 = document.getElementById("output5");
    const output6 = document.getElementById("output6");
    const output7 = document.getElementById("output7");
    function update() {
        if (input.value.match(/\/github.com\/[^\/]+\/[^\/]+/)) {
            const url = input.value.replace(/\/$/, "");

            const filename = ".github%2Fworkflows%2Fverify.yml"
            const value = "name%3A%20verify%0A%0Aon%3A%20push%0A%0Ajobs%3A%0A%20%20build%3A%0A%20%20%20%20runs-on%3A%20ubuntu-latest%0A%0A%20%20%20%20steps%3A%0A%20%20%20%20-%20uses%3A%20actions%2Fcheckout%40v1%0A%0A%20%20%20%20-%20name%3A%20Set%20up%20Python%0A%20%20%20%20%20%20uses%3A%20actions%2Fsetup-python%40v1%0A%0A%20%20%20%20-%20name%3A%20Install%20dependencies%0A%20%20%20%20%20%20run%3A%20pip3%20install%20-U%20git%2Bhttps%3A%2F%2Fgithub.com%2Fkmyk%2Fonline-judge-verify-helper.git%40master%0A%0A%20%20%20%20-%20name%3A%20Run%20tests%0A%20%20%20%20%20%20env%3A%0A%20%20%20%20%20%20%20%20GITHUB_TOKEN%3A%20%24%7B%7B%20secrets.GITHUB_TOKEN%20%7D%7D%0A%20%20%20%20%20%20%20%20GH_PAT%3A%20%24%7B%7B%20secrets.GH_PAT%20%7D%7D%0A%20%20%20%20%20%20run%3A%20oj-verify%20all";
            output.href = url + "/new/master?filename=" + filename + "&value=" + value;
            output.textContent = url + "&value=...";

            const filename2 = "example.test.cpp";
            const value2 = "%23define%20PROBLEM%20%22https%3A%2F%2Fonlinejudge.u-aizu.ac.jp%2Fcourses%2Flesson%2F1%2FALDS1%2F4%2FALDS1_4_B%22%0A%23include%20%3Calgorithm%3E%0A%23include%20%3Ciostream%3E%0A%23include%20%3Cvector%3E%0A%23define%20REP%28i%2C%20n%29%20for%20%28int%20i%20%3D%200%3B%20%28i%29%20%3C%20%28int%29%28n%29%3B%20%2B%2B%20%28i%29%29%0A%23define%20ALL%28x%29%20std%3A%3Abegin%28x%29%2C%20std%3A%3Aend%28x%29%0Ausing%20namespace%20std%3B%0A%0Aint%20main%28%29%20%7B%0A%20%20%20%20int%20n%3B%20cin%20%3E%3E%20n%3B%0A%20%20%20%20vector%3Cint%3E%20s%28n%29%3B%0A%20%20%20%20REP%20%28i%2C%20n%29%20%7B%0A%20%20%20%20%20%20%20%20cin%20%3E%3E%20s%5Bi%5D%3B%0A%20%20%20%20%7D%0A%20%20%20%20int%20q%3B%20cin%20%3E%3E%20q%3B%0A%20%20%20%20int%20cnt%20%3D%200%3B%0A%20%20%20%20while%20%28q%20--%29%20%7B%0A%20%20%20%20%20%20%20%20int%20t_i%3B%20cin%20%3E%3E%20t_i%3B%0A%20%20%20%20%20%20%20%20cnt%20%2B%3D%20binary_search%28ALL%28s%29%2C%20t_i%29%3B%0A%20%20%20%20%7D%0A%20%20%20%20cout%20%3C%3C%20cnt%20%3C%3C%20endl%3B%0A%20%20%20%20return%200%3B%0A%7D";
            output2.href = url + "/new/master?filename=" + filename2 + "&value=" + value2;

            output3.href = input.value.replace(/\/$/, "") + "/actions";
            output5.href = input.value.replace(/\/$/, "") + "/actions";

            output4.textContent = "[![Actions Status](" + url + "/workflows/verify/badge.svg)](" + url + "/actions)";
            output6.src = url + "/workflows/verify/badge.svg";
            output7.src = url + "/workflows/verify/badge.svg";
        }
    }
    input.addEventListener('change', update);
    input.addEventListener('keyup', update);
    update();

    // workaround for the Dinky theme
    output6.margin = 0;
    output6.padding = 0;
    output7.margin = 0;
    output7.padding = 0;
</script>