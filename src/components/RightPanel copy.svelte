<script>
    import { writable } from 'svelte/store';

    // Svelteのストアを利用して状態を管理します
    // (元のコードに合わせていますが、通常はストアを使うことが推奨されます)
    const gridState = writable(Array(64).fill(0));

    // パターンの数（色の数）
    const MAX_BUTTON_VALUE = 12;

    // ボタンの色を定義する配列（パステルカラー）
    const pastelColors = [
        '#f9f9f9',    // 0: デフォルト
        '#ffcdd2',    // 1: ライトピンク
        '#f8bbd0',    // 2: ピンク
        '#e1bee7',    // 3: ラベンダー
        '#d1c4e9',    // 4: ライトパープル
        '#c5cae9',    // 5: インディゴライト
        '#bbdefb',    // 6: ライトブルー
        '#b3e5fc',    // 7: シアンライト
        '#b2ebf2',    // 8: ティールライト
        '#b2dfdb',    // 9: グリーンライト
        '#c8e6c9',    // 10: ライトグリーン
        '#dcedc8',    // 11: ライムライト
        '#f0f4c3'     // 12: イエローライト
    ];

    // ボタンクリック時の処理
    function handleClick(index) {
        $gridState[index] = ($gridState[index] >= MAX_BUTTON_VALUE) ? 0 : $gridState[index] + 1;
        // ストアの値を更新
        gridState.set($gridState);
    }
</script>

<div class="container">
    <!-- 8x8の表にボタンを埋め込むことで8x8のボタンのグリッドを作成する -->
    <table>
        <tbody>
            {#each Array(8) as _, row}
                <tr>
                    {#each Array(8) as _, col}
                        {@const index = row * 8 + col}
                        <td>
                            <button
                                on:click={() => handleClick(index)}
                                style="background-color: {pastelColors[$gridState[index]]};"
                            >
                                {#if $gridState[index] > 0}
                                    <span class="heart">❤️</span>
                                {/if}
                            </button>
                        </td>
                    {/each}
                </tr>
            {/each}
        </tbody>
    </table>
</div>

<style>
    .container {
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 100vh;
        background-color: #fff0f5; /* 背景を薄いピンク色に */
    }

    button {
        width: 45px;
        height: 45px;
        font-size: 22px;
        border: none; /* 枠線をなくす */
        background-color: #f9f9f9;
        cursor: pointer;
        border-radius: 12px; /* 角を丸くして柔らかい印象に */
        transition: all 0.2s ease;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1); /* 少し影をつける */
        display: flex;
        justify-content: center;
        align-items: center;
    }

    button:hover {
        transform: scale(1.1); /* ホバー時に少し大きく */
    }

    button:active {
        transform: scale(0.95); /* クリック時に少し小さく */
        box-shadow: 0 1px 2px rgba(0,0,0,0.1);
    }

    .heart {
        animation: pop 0.3s ease;
    }

    /* ハートが出現する時のアニメーション */
    @keyframes pop {
        0% {
            transform: scale(0);
            opacity: 0;
        }
        50% {
            transform: scale(1.2);
        }
        100% {
            transform: scale(1);
            opacity: 1;
        }
    }

    table {
        border-collapse: separate; /* セルを分離 */
        border-spacing: 5px; /* ボタンの間に少し隙間を空ける */
    }

    td {
        padding: 0;
        border: none;
    }
</style>