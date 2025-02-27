# NLP
This  is the capstone project
{
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "colab_type": "text",
        "id": "view-in-github"
      },
      "source": [
        "<a href=\"https://colab.research.google.com/github/meahmadkarim/Topical_Chatbot/blob/main/Topical_Chatbot_ipynb.ipynb\" target=\"_parent\"><img src=\"https://colab.research.google.com/assets/colab-badge.svg\" alt=\"Open In Colab\"/></a>"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "zQ8QC3AjRp0E",
        "outputId": "a6dce2fa-8dca-4e1d-e42d-2df2054baad1"
      },
      "outputs": [
        {
          "name": "stdout",
          "output_type": "stream",
          "text": [
            "Mounted at /content/drive\n"
          ]
        }
      ],
      "source": [
        "from google.colab import drive\n",
        "drive.mount('/content/drive')"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "x3l9lF9MRzXU",
        "outputId": "7795de3e-c498-424e-dc71-e5fbfc3ec483"
      },
      "outputs": [
        {
          "name": "stdout",
          "output_type": "stream",
          "text": [
            "Requirement already satisfied: pandas in /usr/local/lib/python3.10/dist-packages (1.5.3)\n",
            "Requirement already satisfied: nltk in /usr/local/lib/python3.10/dist-packages (3.8.1)\n",
            "Requirement already satisfied: python-dateutil>=2.8.1 in /usr/local/lib/python3.10/dist-packages (from pandas) (2.8.2)\n",
            "Requirement already satisfied: pytz>=2020.1 in /usr/local/lib/python3.10/dist-packages (from pandas) (2023.3)\n",
            "Requirement already satisfied: numpy>=1.21.0 in /usr/local/lib/python3.10/dist-packages (from pandas) (1.23.5)\n",
            "Requirement already satisfied: click in /usr/local/lib/python3.10/dist-packages (from nltk) (8.1.7)\n",
            "Requirement already satisfied: joblib in /usr/local/lib/python3.10/dist-packages (from nltk) (1.3.2)\n",
            "Requirement already satisfied: regex>=2021.8.3 in /usr/local/lib/python3.10/dist-packages (from nltk) (2023.6.3)\n",
            "Requirement already satisfied: tqdm in /usr/local/lib/python3.10/dist-packages (from nltk) (4.66.1)\n",
            "Requirement already satisfied: six>=1.5 in /usr/local/lib/python3.10/dist-packages (from python-dateutil>=2.8.1->pandas) (1.16.0)\n"
          ]
        }
      ],
      "source": [
        "pip install pandas nltk"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "wv_QMYGASMZt"
      },
      "outputs": [],
      "source": [
        "import numpy as np\n",
        "import tensorflow as tf\n",
        "from tensorflow import keras\n",
        "import string, os\n",
        "import re\n",
        "from tqdm import tqdm\n",
        "import pandas as pd\n",
        "import matplotlib.pyplot as plt\n",
        "import pickle"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 293
        },
        "id": "BTXSBNVYSN9T",
        "outputId": "7440595d-9d5d-4c42-eaf4-56872e2ec5ff"
      },
      "outputs": [
        {
          "data": {
            "text/html": [
              "\n",
              "  <div id=\"df-ab96cd68-9c45-46b7-b625-014da16d98f7\" class=\"colab-df-container\">\n",
              "    <div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>conversation_id</th>\n",
              "      <th>message</th>\n",
              "      <th>sentiment</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>1</td>\n",
              "      <td>Are you a fan of Google or Microsoft?</td>\n",
              "      <td>Curious to dive deeper</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>1</td>\n",
              "      <td>Both are excellent technology they are helpfu...</td>\n",
              "      <td>Curious to dive deeper</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>1</td>\n",
              "      <td>I'm not  a huge fan of Google, but I use it a...</td>\n",
              "      <td>Curious to dive deeper</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>1</td>\n",
              "      <td>Google provides online related services and p...</td>\n",
              "      <td>Curious to dive deeper</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>1</td>\n",
              "      <td>Yeah, their services are good. I'm just not a...</td>\n",
              "      <td>Curious to dive deeper</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "    <div class=\"colab-df-buttons\">\n",
              "\n",
              "  <div class=\"colab-df-container\">\n",
              "    <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-ab96cd68-9c45-46b7-b625-014da16d98f7')\"\n",
              "            title=\"Convert this dataframe to an interactive table.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\" viewBox=\"0 -960 960 960\">\n",
              "    <path d=\"M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "\n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    .colab-df-buttons div {\n",
              "      margin-bottom: 4px;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "    <script>\n",
              "      const buttonEl =\n",
              "        document.querySelector('#df-ab96cd68-9c45-46b7-b625-014da16d98f7 button.colab-df-convert');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      async function convertToInteractive(key) {\n",
              "        const element = document.querySelector('#df-ab96cd68-9c45-46b7-b625-014da16d98f7');\n",
              "        const dataTable =\n",
              "          await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                    [key], {});\n",
              "        if (!dataTable) return;\n",
              "\n",
              "        const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "          '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "          + ' to learn more about interactive tables.';\n",
              "        element.innerHTML = '';\n",
              "        dataTable['output_type'] = 'display_data';\n",
              "        await google.colab.output.renderOutput(dataTable, element);\n",
              "        const docLink = document.createElement('div');\n",
              "        docLink.innerHTML = docLinkHtml;\n",
              "        element.appendChild(docLink);\n",
              "      }\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "\n",
              "<div id=\"df-6fd7cfd7-56da-4502-ac50-5427fdac43bb\">\n",
              "  <button class=\"colab-df-quickchart\" onclick=\"quickchart('df-6fd7cfd7-56da-4502-ac50-5427fdac43bb')\"\n",
              "            title=\"Suggest charts.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "<svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "     width=\"24px\">\n",
              "    <g>\n",
              "        <path d=\"M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z\"/>\n",
              "    </g>\n",
              "</svg>\n",
              "  </button>\n",
              "\n",
              "<style>\n",
              "  .colab-df-quickchart {\n",
              "      --bg-color: #E8F0FE;\n",
              "      --fill-color: #1967D2;\n",
              "      --hover-bg-color: #E2EBFA;\n",
              "      --hover-fill-color: #174EA6;\n",
              "      --disabled-fill-color: #AAA;\n",
              "      --disabled-bg-color: #DDD;\n",
              "  }\n",
              "\n",
              "  [theme=dark] .colab-df-quickchart {\n",
              "      --bg-color: #3B4455;\n",
              "      --fill-color: #D2E3FC;\n",
              "      --hover-bg-color: #434B5C;\n",
              "      --hover-fill-color: #FFFFFF;\n",
              "      --disabled-bg-color: #3B4455;\n",
              "      --disabled-fill-color: #666;\n",
              "  }\n",
              "\n",
              "  .colab-df-quickchart {\n",
              "    background-color: var(--bg-color);\n",
              "    border: none;\n",
              "    border-radius: 50%;\n",
              "    cursor: pointer;\n",
              "    display: none;\n",
              "    fill: var(--fill-color);\n",
              "    height: 32px;\n",
              "    padding: 0;\n",
              "    width: 32px;\n",
              "  }\n",
              "\n",
              "  .colab-df-quickchart:hover {\n",
              "    background-color: var(--hover-bg-color);\n",
              "    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "    fill: var(--button-hover-fill-color);\n",
              "  }\n",
              "\n",
              "  .colab-df-quickchart-complete:disabled,\n",
              "  .colab-df-quickchart-complete:disabled:hover {\n",
              "    background-color: var(--disabled-bg-color);\n",
              "    fill: var(--disabled-fill-color);\n",
              "    box-shadow: none;\n",
              "  }\n",
              "\n",
              "  .colab-df-spinner {\n",
              "    border: 2px solid var(--fill-color);\n",
              "    border-color: transparent;\n",
              "    border-bottom-color: var(--fill-color);\n",
              "    animation:\n",
              "      spin 1s steps(1) infinite;\n",
              "  }\n",
              "\n",
              "  @keyframes spin {\n",
              "    0% {\n",
              "      border-color: transparent;\n",
              "      border-bottom-color: var(--fill-color);\n",
              "      border-left-color: var(--fill-color);\n",
              "    }\n",
              "    20% {\n",
              "      border-color: transparent;\n",
              "      border-left-color: var(--fill-color);\n",
              "      border-top-color: var(--fill-color);\n",
              "    }\n",
              "    30% {\n",
              "      border-color: transparent;\n",
              "      border-left-color: var(--fill-color);\n",
              "      border-top-color: var(--fill-color);\n",
              "      border-right-color: var(--fill-color);\n",
              "    }\n",
              "    40% {\n",
              "      border-color: transparent;\n",
              "      border-right-color: var(--fill-color);\n",
              "      border-top-color: var(--fill-color);\n",
              "    }\n",
              "    60% {\n",
              "      border-color: transparent;\n",
              "      border-right-color: var(--fill-color);\n",
              "    }\n",
              "    80% {\n",
              "      border-color: transparent;\n",
              "      border-right-color: var(--fill-color);\n",
              "      border-bottom-color: var(--fill-color);\n",
              "    }\n",
              "    90% {\n",
              "      border-color: transparent;\n",
              "      border-bottom-color: var(--fill-color);\n",
              "    }\n",
              "  }\n",
              "</style>\n",
              "\n",
              "  <script>\n",
              "    async function quickchart(key) {\n",
              "      const quickchartButtonEl =\n",
              "        document.querySelector('#' + key + ' button');\n",
              "      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.\n",
              "      quickchartButtonEl.classList.add('colab-df-spinner');\n",
              "      try {\n",
              "        const charts = await google.colab.kernel.invokeFunction(\n",
              "            'suggestCharts', [key], {});\n",
              "      } catch (error) {\n",
              "        console.error('Error during call to suggestCharts:', error);\n",
              "      }\n",
              "      quickchartButtonEl.classList.remove('colab-df-spinner');\n",
              "      quickchartButtonEl.classList.add('colab-df-quickchart-complete');\n",
              "    }\n",
              "    (() => {\n",
              "      let quickchartButtonEl =\n",
              "        document.querySelector('#df-6fd7cfd7-56da-4502-ac50-5427fdac43bb button');\n",
              "      quickchartButtonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "    })();\n",
              "  </script>\n",
              "</div>\n",
              "    </div>\n",
              "  </div>\n"
            ],
            "text/plain": [
              "   conversation_id                                            message  \\\n",
              "0                1              Are you a fan of Google or Microsoft?   \n",
              "1                1   Both are excellent technology they are helpfu...   \n",
              "2                1   I'm not  a huge fan of Google, but I use it a...   \n",
              "3                1   Google provides online related services and p...   \n",
              "4                1   Yeah, their services are good. I'm just not a...   \n",
              "\n",
              "                 sentiment  \n",
              "0   Curious to dive deeper  \n",
              "1   Curious to dive deeper  \n",
              "2   Curious to dive deeper  \n",
              "3   Curious to dive deeper  \n",
              "4   Curious to dive deeper  "
            ]
          },
          "execution_count": 5,
          "metadata": {},
          "output_type": "execute_result"
        }
      ],
      "source": [
        "# Read the CSV file into a DataFrame\n",
        "df = pd.read_csv('/content/drive/MyDrive/NLP Project/Data/topical_chat.csv')\n",
        "df.head()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "vaA9CCBJSN6w"
      },
      "outputs": [],
      "source": [
        "import string\n",
        "\n",
        "def custom_preprocess(text):\n",
        "    # Convert to lowercase\n",
        "    text = text.lower()\n",
        "\n",
        "    # Replace specific characters with spaces\n",
        "    for char in ['\\n', '-', ':', ',', '\"', '.', '!', '?', ';']:\n",
        "        text = text.replace(char, ' ')\n",
        "\n",
        "    # Remove punctuation characters\n",
        "    text = ''.join(char for char in text if char not in string.punctuation)\n",
        "\n",
        "    # Remove extra spaces\n",
        "    text = ' '.join(text.split())\n",
        "\n",
        "    return text\n"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "OAQCW2iwSN4v"
      },
      "outputs": [],
      "source": [
        "df.message = df.message.apply(custom_preprocess)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "l8iubKHqSN2j",
        "outputId": "96543a0d-e713-4144-bff4-bb3732fe2650"
      },
      "outputs": [
        {
          "name": "stderr",
          "output_type": "stream",
          "text": [
            "100%|██████████| 188378/188378 [00:42<00:00, 4468.91it/s]\n"
          ]
        }
      ],
      "source": [
        "# Vectorize the data.\n",
        "X = []\n",
        "Y = []\n",
        "input_words_set = set()\n",
        "target_words_set = set()\n",
        "\n",
        "for conversation_index in tqdm(range(df.shape[0])):\n",
        "\n",
        "    if conversation_index == 0:\n",
        "        continue\n",
        "\n",
        "    x = df.iloc[conversation_index - 1]\n",
        "    y = df.iloc[conversation_index]\n",
        "\n",
        "    if x.conversation_id == y.conversation_id:\n",
        "\n",
        "        x = x.message\n",
        "        y = y.message\n",
        "\n",
        "        if len(x.split()) > 2 and \\\n",
        "            len(y.split()) > 0 and \\\n",
        "            len(x.split()) < 30 and \\\n",
        "            len(y.split()) < 10 and \\\n",
        "            x and \\\n",
        "            y:\n",
        "\n",
        "            y = \"str  \" + y + \" end\"\n",
        "\n",
        "            X.append(x)\n",
        "            Y.append(y)\n",
        "\n",
        "            for word in x.split():\n",
        "                if word not in input_words_set:\n",
        "                    input_words_set.add(word)\n",
        "            for word in y.split():\n",
        "                if word not in target_words_set:\n",
        "                    target_words_set.add(word)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "3kB9VYq_SNz9"
      },
      "outputs": [],
      "source": [
        "import tensorflow_datasets as tfds\n",
        "\n",
        "import os\n",
        "import re\n",
        "import numpy as np\n",
        "import pandas as pd\n",
        "import math\n",
        "\n",
        "import matplotlib.pyplot as plt\n",
        "MAX_SENTENCE_LENGTH = 60"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "0yxttRNOSNxN"
      },
      "outputs": [],
      "source": [
        "import tensorflow_datasets as tfds\n",
        "\n",
        "# Your data preparation code here\n",
        "# ...\n",
        "\n",
        "tokenizer = tfds.deprecated.text.SubwordTextEncoder.build_from_corpus(\n",
        "    X + Y, target_vocab_size=2**13)\n",
        "\n",
        "START_TOKEN, END_TOKEN = [tokenizer.vocab_size], [tokenizer.vocab_size + 1]\n"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "2jkt6ya3SNuV"
      },
      "outputs": [],
      "source": [
        "def tokenize_and_filter(X, Y):\n",
        "  tok_input, tok_output = [], []\n",
        "\n",
        "  for (sen, sen1) in zip(X, Y):\n",
        "    # tokenize sentence\n",
        "    sen = START_TOKEN + tokenizer.encode(sen) + END_TOKEN\n",
        "    sen1 = START_TOKEN + tokenizer.encode(sen1) + END_TOKEN\n",
        "    # check tokenized sentence max length\n",
        "    if len(sen) <= MAX_SENTENCE_LENGTH and len(sen1) <= MAX_SENTENCE_LENGTH:\n",
        "      tok_input.append(sen)\n",
        "      tok_output.append(sen1)\n",
        "\n",
        "  # pad tokenized sentences\n",
        "  tok_input = tf.keras.preprocessing.sequence.pad_sequences(\n",
        "      tok_input, maxlen=MAX_SENTENCE_LENGTH, padding='post')\n",
        "  tok_output = tf.keras.preprocessing.sequence.pad_sequences(\n",
        "      tok_output, maxlen=MAX_SENTENCE_LENGTH, padding='post')\n",
        "\n",
        "  return tok_input, tok_output\n",
        "\n",
        "\n",
        "X, Y = tokenize_and_filter(X, Y)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "rhUl6_2cZRh-"
      },
      "outputs": [],
      "source": [
        "BATCH_SIZE = 64\n",
        "BUFFER_SIZE = 20000\n",
        "\n",
        "dataset = tf.data.Dataset.from_tensor_slices((\n",
        "    {\n",
        "        'inputs': X,\n",
        "        'dec_inputs': Y[:, :-1]\n",
        "    },\n",
        "    {\n",
        "        'outputs': Y[:, 1:]\n",
        "    },\n",
        "))\n",
        "\n",
        "dataset = dataset.cache()\n",
        "dataset = dataset.shuffle(BUFFER_SIZE)\n",
        "dataset = dataset.batch(BATCH_SIZE)\n",
        "dataset = dataset.prefetch(tf.data.experimental.AUTOTUNE)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "_4PmbXIGZReM"
      },
      "outputs": [],
      "source": [
        "def scaled_attention(query, key, value, mask):\n",
        "  \"\"\"Calculate the attention weights. \"\"\"\n",
        "  matmul_qk = tf.matmul(query, key, transpose_b=True)\n",
        "\n",
        "  # scale matmul_qk\n",
        "  depth = tf.cast(tf.shape(key)[-1], tf.float32)\n",
        "  logits = matmul_qk / tf.math.sqrt(depth)\n",
        "\n",
        "  # add the mask to zero out padding tokens\n",
        "  if mask is not None:\n",
        "    logits += (mask * -1e9)\n",
        "\n",
        "  # softmax is normalized on the last axis (seq_len_k)\n",
        "  weight = tf.nn.softmax(logits, axis=-1)\n",
        "\n",
        "  output = tf.matmul(weight, value)\n",
        "\n",
        "  return output"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "ui5ZTS7CZRam"
      },
      "outputs": [],
      "source": [
        "class MultiHeadAttention(tf.keras.layers.Layer):\n",
        "\n",
        "  def __init__(self, d_model, num_heads, name=\"multi_head_attention\"):\n",
        "    super(MultiHeadAttention, self).__init__(name=name)\n",
        "    self.num_heads = num_heads\n",
        "    self.d_model = d_model\n",
        "\n",
        "    assert d_model % self.num_heads == 0\n",
        "\n",
        "    self.depth = d_model // self.num_heads\n",
        "\n",
        "    self.query_dense = tf.keras.layers.Dense(units=d_model)\n",
        "    self.key_dense = tf.keras.layers.Dense(units=d_model)\n",
        "    self.value_dense = tf.keras.layers.Dense(units=d_model)\n",
        "\n",
        "    self.dense = tf.keras.layers.Dense(units=d_model)\n",
        "\n",
        "  def split_heads(self, inputs, batch_size):\n",
        "    inputs = tf.reshape(\n",
        "        inputs, shape=(batch_size, -1, self.num_heads, self.depth))\n",
        "    return tf.transpose(inputs, perm=[0, 2, 1, 3])\n",
        "\n",
        "  def call(self, inputs):\n",
        "    query, key, value, mask = inputs['query'], inputs['key'], inputs[\n",
        "        'value'], inputs['mask']\n",
        "    batch_size = tf.shape(query)[0]\n",
        "\n",
        "    # linear layers\n",
        "    query = self.query_dense(query)\n",
        "    key = self.key_dense(key)\n",
        "    value = self.value_dense(value)\n",
        "\n",
        "    # split heads\n",
        "    query = self.split_heads(query, batch_size)\n",
        "    key = self.split_heads(key, batch_size)\n",
        "    value = self.split_heads(value, batch_size)\n",
        "\n",
        "    # scaled dot-product attention\n",
        "    scaled_attention = scaled_attention(query, key, value, mask)\n",
        "\n",
        "    scaled_attention = tf.transpose(scaled_attention, perm=[0, 2, 1, 3])\n",
        "\n",
        "    # concatenation of heads\n",
        "    concat_attention = tf.reshape(scaled_attention,\n",
        "                                  (batch_size, -1, self.d_model))\n",
        "\n",
        "    # final linear layer\n",
        "    outputs = self.dense(concat_attention)\n",
        "\n",
        "    return outputs"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "f7MzHh8eZRXY"
      },
      "outputs": [],
      "source": [
        "def create_padding_mask(x):\n",
        "  mask = tf.cast(tf.math.equal(x, 0), tf.float32)\n",
        "  # (batch_size, 1, 1, sequence length)\n",
        "  return mask[:, tf.newaxis, tf.newaxis, :]"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "9bk2L_pbZRTm"
      },
      "outputs": [],
      "source": [
        "def create_look_ahead_mask(x):\n",
        "  seq_len = tf.shape(x)[1]\n",
        "  look_ahead_mask = 1 - tf.linalg.band_part(tf.ones((seq_len, seq_len)), -1, 0)\n",
        "  padding_mask = create_padding_mask(x)\n",
        "  return tf.maximum(look_ahead_mask, padding_mask)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "PzRWJsDbZRHi"
      },
      "outputs": [],
      "source": [
        "\n",
        "class PositionalEncoding(tf.keras.layers.Layer):\n",
        "\n",
        "  def __init__(self, position, d_model):\n",
        "    super(PositionalEncoding, self).__init__()\n",
        "    self.pos_encoding = self.positional_encoding(position, d_model)\n",
        "\n",
        "  def get_angles(self, position, i, d_model):\n",
        "    angles = 1 / tf.pow(10000, (2 * (i // 2)) / tf.cast(d_model, tf.float32))\n",
        "    return position * angles\n",
        "\n",
        "  def positional_encoding(self, position, d_model):\n",
        "    angle_rads = self.get_angles(\n",
        "        position=tf.range(position, dtype=tf.float32)[:, tf.newaxis],\n",
        "        i=tf.range(d_model, dtype=tf.float32)[tf.newaxis, :],\n",
        "        d_model=d_model)\n",
        "    # apply sin to even index in the array\n",
        "    sines = tf.math.sin(angle_rads[:, 0::2])\n",
        "    # apply cos to odd index in the array\n",
        "    cosines = tf.math.cos(angle_rads[:, 1::2])\n",
        "\n",
        "    pos_encoding = tf.concat([sines, cosines], axis=-1)\n",
        "    pos_encoding = pos_encoding[tf.newaxis, ...]\n",
        "    return tf.cast(pos_encoding, tf.float32)\n",
        "\n",
        "  def call(self, inputs):\n",
        "    return inputs + self.pos_encoding[:, :tf.shape(inputs)[1], :]"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "rDGaIP_UZRDP"
      },
      "outputs": [],
      "source": [
        "def encoder_layer(units, d_model, num_heads, dropout, name=\"encoder_layer\"):\n",
        "  inputs = tf.keras.Input(shape=(None, d_model), name=\"inputs\")\n",
        "  padding_mask = tf.keras.Input(shape=(1, 1, None), name=\"padding_mask\")\n",
        "\n",
        "  attention = MultiHeadAttention(\n",
        "      d_model, num_heads, name=\"attention\")({\n",
        "          'query': inputs,\n",
        "          'key': inputs,\n",
        "          'value': inputs,\n",
        "          'mask': padding_mask\n",
        "      })\n",
        "  attention = tf.keras.layers.Dropout(rate=dropout)(attention)\n",
        "  attention = tf.keras.layers.LayerNormalization(\n",
        "      epsilon=1e-6)(inputs + attention)\n",
        "\n",
        "  outputs = tf.keras.layers.Dense(units=units, activation='relu')(attention)\n",
        "  outputs = tf.keras.layers.Dense(units=d_model)(outputs)\n",
        "  outputs = tf.keras.layers.Dropout(rate=dropout)(outputs)\n",
        "  outputs = tf.keras.layers.LayerNormalization(\n",
        "      epsilon=1e-6)(attention + outputs)\n",
        "\n",
        "  return tf.keras.Model(\n",
        "      inputs=[inputs, padding_mask], outputs=outputs, name=name)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 807
        },
        "id": "zFgPgGv2ZQ_h",
        "outputId": "c7930999-19f0-450e-e6d2-e562a0f2848b"
      },
      "outputs": [
        {
          "ename": "UnboundLocalError",
          "evalue": "ignored",
          "output_type": "error",
          "traceback": [
            "\u001b[0;31m---------------------------------------------------------------------------\u001b[0m",
            "\u001b[0;31mUnboundLocalError\u001b[0m                         Traceback (most recent call last)",
            "\u001b[0;32m<ipython-input-24-6c3e54673f4b>\u001b[0m in \u001b[0;36m<cell line: 1>\u001b[0;34m()\u001b[0m\n\u001b[0;32m----> 1\u001b[0;31m sample_encoder_layer = encoder_layer(\n\u001b[0m\u001b[1;32m      2\u001b[0m     \u001b[0munits\u001b[0m\u001b[0;34m=\u001b[0m\u001b[0;36m512\u001b[0m\u001b[0;34m,\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[1;32m      3\u001b[0m     \u001b[0md_model\u001b[0m\u001b[0;34m=\u001b[0m\u001b[0;36m256\u001b[0m\u001b[0;34m,\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[1;32m      4\u001b[0m     \u001b[0mnum_heads\u001b[0m\u001b[0;34m=\u001b[0m\u001b[0;36m8\u001b[0m\u001b[0;34m,\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[1;32m      5\u001b[0m     \u001b[0mdropout\u001b[0m\u001b[0;34m=\u001b[0m\u001b[0;36m0.1\u001b[0m\u001b[0;34m,\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n",
            "\u001b[0;32m<ipython-input-21-357ca53de1c0>\u001b[0m in \u001b[0;36mencoder_layer\u001b[0;34m(units, d_model, num_heads, dropout, name)\u001b[0m\n\u001b[1;32m      3\u001b[0m   \u001b[0mpadding_mask\u001b[0m \u001b[0;34m=\u001b[0m \u001b[0mtf\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mkeras\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mInput\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mshape\u001b[0m\u001b[0;34m=\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0;36m1\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0;36m1\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0;32mNone\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mname\u001b[0m\u001b[0;34m=\u001b[0m\u001b[0;34m\"padding_mask\"\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[1;32m      4\u001b[0m \u001b[0;34m\u001b[0m\u001b[0m\n\u001b[0;32m----> 5\u001b[0;31m   attention = MultiHeadAttention(\n\u001b[0m\u001b[1;32m      6\u001b[0m       d_model, num_heads, name=\"attention\")({\n\u001b[1;32m      7\u001b[0m           \u001b[0;34m'query'\u001b[0m\u001b[0;34m:\u001b[0m \u001b[0minputs\u001b[0m\u001b[0;34m,\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n",
            "\u001b[0;32m/usr/local/lib/python3.10/dist-packages/keras/utils/traceback_utils.py\u001b[0m in \u001b[0;36merror_handler\u001b[0;34m(*args, **kwargs)\u001b[0m\n\u001b[1;32m     68\u001b[0m             \u001b[0;31m# To get the full stack trace, call:\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[1;32m     69\u001b[0m             \u001b[0;31m# `tf.debugging.disable_traceback_filtering()`\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[0;32m---> 70\u001b[0;31m             \u001b[0;32mraise\u001b[0m \u001b[0me\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mwith_traceback\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mfiltered_tb\u001b[0m\u001b[0;34m)\u001b[0m \u001b[0;32mfrom\u001b[0m \u001b[0;32mNone\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[0m\u001b[1;32m     71\u001b[0m         \u001b[0;32mfinally\u001b[0m\u001b[0;34m:\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[1;32m     72\u001b[0m             \u001b[0;32mdel\u001b[0m \u001b[0mfiltered_tb\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n",
            "\u001b[0;32m/tmp/__autograph_generated_file7qhpogly.py\u001b[0m in \u001b[0;36mtf__call\u001b[0;34m(self, inputs)\u001b[0m\n\u001b[1;32m     16\u001b[0m                 \u001b[0mkey\u001b[0m \u001b[0;34m=\u001b[0m \u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mconverted_call\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mld\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mself\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0msplit_heads\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0;34m(\u001b[0m\u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mld\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mkey\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mld\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mbatch_size\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0;32mNone\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mfscope\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[1;32m     17\u001b[0m                 \u001b[0mvalue\u001b[0m \u001b[0;34m=\u001b[0m \u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mconverted_call\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mld\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mself\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0msplit_heads\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0;34m(\u001b[0m\u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mld\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mvalue\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mld\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mbatch_size\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0;32mNone\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mfscope\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[0;32m---> 18\u001b[0;31m                 \u001b[0mscaled_attention\u001b[0m \u001b[0;34m=\u001b[0m \u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mconverted_call\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mld\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mscaled_attention\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0;34m(\u001b[0m\u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mld\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mquery\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mld\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mkey\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mld\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mvalue\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mld\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mmask\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0;32mNone\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mfscope\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[0m\u001b[1;32m     19\u001b[0m                 \u001b[0mscaled_attention\u001b[0m \u001b[0;34m=\u001b[0m \u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mconverted_call\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mld\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mtf\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mtranspose\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0;34m(\u001b[0m\u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mld\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mscaled_attention\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m,\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mdict\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mperm\u001b[0m\u001b[0;34m=\u001b[0m\u001b[0;34m[\u001b[0m\u001b[0;36m0\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0;36m2\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0;36m1\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0;36m3\u001b[0m\u001b[0;34m]\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mfscope\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[1;32m     20\u001b[0m                 \u001b[0mconcat_attention\u001b[0m \u001b[0;34m=\u001b[0m \u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mconverted_call\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mld\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mtf\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mreshape\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0;34m(\u001b[0m\u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mld\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mscaled_attention\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0;34m(\u001b[0m\u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mld\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mbatch_size\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0;34m-\u001b[0m\u001b[0;36m1\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mag__\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0mld\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mself\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m.\u001b[0m\u001b[0md_model\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0;32mNone\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mfscope\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n",
            "\u001b[0;31mUnboundLocalError\u001b[0m: Exception encountered when calling layer \"attention\" (type MultiHeadAttention).\n\nin user code:\n\n    File \"<ipython-input-17-7ccc7c3b013b>\", line 39, in call  *\n        scaled_attention = scaled_attention(query, key, value, mask)\n\n    UnboundLocalError: local variable 'scaled_attention' referenced before assignment\n\n\nCall arguments received by layer \"attention\" (type MultiHeadAttention):\n  • inputs={'query': 'tf.Tensor(shape=(None, None, 256), dtype=float32)', 'key': 'tf.Tensor(shape=(None, None, 256), dtype=float32)', 'value': 'tf.Tensor(shape=(None, None, 256), dtype=float32)', 'mask': 'tf.Tensor(shape=(None, 1, 1, None), dtype=float32)'}"
          ]
        }
      ],
      "source": [
        "sample_encoder_layer = encoder_layer(\n",
        "    units=512,\n",
        "    d_model=256,\n",
        "    num_heads=8,\n",
        "    dropout=0.1,\n",
        "    name=\"our_encoder_layer\")\n",
        "\n",
        "tf.keras.utils.plot_model(\n",
        "    sample_encoder_layer, to_file='encoder_layer.png', show_shapes=True)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "7w1_dU-FZmD3"
      },
      "outputs": [],
      "source": []
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "haNsy6diZmBN"
      },
      "outputs": [],
      "source": []
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "0BCyaZxuZl-S"
      },
      "outputs": [],
      "source": []
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "JTiccnT8Zl7l"
      },
      "outputs": [],
      "source": []
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "RQcNI3qrZl4u"
      },
      "outputs": [],
      "source": []
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "e336qUqQZl2H"
      },
      "outputs": [],
      "source": []
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "dTrxRFDSZlzf"
      },
      "outputs": [],
      "source": []
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "v-OAC5AiZlwn"
      },
      "outputs": [],
      "source": []
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "BtKuoefiZlta"
      },
      "outputs": [],
      "source": []
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "bzcNOoMZZlpw"
      },
      "outputs": [],
      "source": []
    }
  ],
  "metadata": {
    "colab": {
      "authorship_tag": "ABX9TyPjc1UHOojJ5UKT6DLjCNjj",
      "include_colab_link": true,
      "provenance": []
    },
    "kernelspec": {
      "display_name": "Python 3",
      "name": "python3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "nbformat": 4,
  "nbformat_minor": 0
}

